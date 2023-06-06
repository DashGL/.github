---
title: "Pcsx-Reloaded Memory Debug Edition"
description: "Editing the code of PCSX Reloaded to store save states in an uncompressed format"
pubDate: "May 5, 2020"
heroImage: "/img/PCSX-Reloaded-captura-de-tela-4-baixesoft-9398709.png"
author: Kion
---

I’ve finally had a tiny bit of time to put into looking into PSX graphics again. And I think it would be a good idea to fork pcsx-r to try and re-write the save state writing and save state loading to make it easier to debug system memory. The cloned version is over at [https://gitlab.com/kion-dgl/pcsxr-mem](https://gitlab.com/kion-dgl/pcsxr-mem).

The code responsible for reading and writing states is `libpcsxcore/misc.c`.

```c
int SaveState(const char \*file) {
    gzFile f;
    long size;

    f = gzopen(file, "wb9"); // Best ratio but slow
    if (f == NULL) return -1;
    return SaveStateGz(f, &size);
}

int LoadState(const char \*file) {
    gzFile f;

    f = gzopen(file, "rb");
    if (f == NULL) return -1;
    return LoadStateGz(f);
}
```

We have two functions SaveState and LoadState. And they both use a gxFile object type to create a compressed snapshot of memory to save as a state. We really don’t need this to be compressed. An easy change would probably be to change the compression from a 9 to a 0 for no compression. But I’m also tempted to change the order the file is written to make editing and reading easier.

```c
int SaveStateGz(gzFile f, long\* gzsize) {
	int Size;
	unsigned char pMemGpuPic\[SZ\_GPUPIC\];

	//if (f == NULL) return -1;

	gzwrite(f, (void \*)PcsxrHeader, sizeof(PcsxrHeader));
	gzwrite(f, (void \*)&SaveVersion, sizeof(u32));
	gzwrite(f, (void \*)&Config.HLE, sizeof(boolean));

	if (gzsize)GPU\_getScreenPic(pMemGpuPic); // Not necessary with ephemeral saves
	gzwrite(f, pMemGpuPic, SZ\_GPUPIC);

	if (Config.HLE)
		psxBiosFreeze(1);

	gzwrite(f, psxM, 0x00200000);
	gzwrite(f, psxR, 0x00080000);
	gzwrite(f, psxH, 0x00010000);
	gzwrite(f, (void \*)&psxRegs, sizeof(psxRegs));

	// gpu
	if (!gpufP)gpufP = (GPUFreeze\_t \*)malloc(sizeof(GPUFreeze\_t));
	gpufP->ulFreezeVersion = 1;
	GPU\_freeze(1, gpufP);
	gzwrite(f, gpufP, sizeof(GPUFreeze\_t));

	// SPU Plugin cannot change during run, so we query size info just once per session
	if (!spufP) {
		spufP = (SPUFreeze\_t \*)malloc(offsetof(SPUFreeze\_t, SPUPorts)); // only first 3 elements (up to Size)        
		SPU\_freeze(2, spufP);
		Size = spufP->Size;
		SysPrintf("SPUFreezeSize %i/(%i)\\n", Size, offsetof(SPUFreeze\_t, SPUPorts));
		free(spufP);
		spufP = (SPUFreeze\_t \*) malloc(Size);
		spufP->Size = Size;

		if (spufP->Size <= 0) {
			gzclose(f);
			free(spufP);
			spufP = NULL;
			return 1; // error
		}
	}
	// spu
	gzwrite(f, &(spufP->Size), 4);
	SPU\_freeze(1, spufP);
	gzwrite(f, spufP, spufP->Size);

	sioFreeze(f, 1);
	cdrFreeze(f, 1);
	psxHwFreeze(f, 1);
	psxRcntFreeze(f, 1);
	mdecFreeze(f, 1);

	if(gzsize)\*gzsize = gztell(f);
	gzclose(f);

	return 0;
}
```

Here is the save state. And I think we’re interested in system memory and the framebuffer specifically. So we we’re interested in:

```c
gzwrite(f, psxM, 0x00200000); 
...
// gpu
if (!gpufP)gpufP = (GPUFreeze\_t \*)malloc(sizeof(GPUFreeze\_t));
gpufP->ulFreezeVersion = 1;
GPU\_freeze(1, gpufP);
gzwrite(f, gpufP, sizeof(GPUFreeze\_t));

And I think if we turn off compression and move these to the start of the function, we should get an uncompressed save state with memory for the first 2MB, followed by vram for the next 1MB, and then everything else thrown in after that. But then we need to make sure the Load state function also matches.

int LoadStateGz(gzFile f) {
	SPUFreeze\_t \*\_spufP;
	int Size;
	char header\[sizeof(PcsxrHeader)\];
	u32 version;
	boolean hle;

	if (f == NULL) return -1;

	gzread(f, header, sizeof(header));
	gzread(f, &version, sizeof(u32));
	gzread(f, &hle, sizeof(boolean));

	// Compare header only "STv4 PCSXR" part no version
	if (strncmp(PcsxrHeader, header, PCSXR\_HEADER\_SZ) != 0 || version != SaveVersion || hle != Config.HLE) {
		gzclose(f);
		return -1;
	}

	psxCpu->Reset();
	gzseek(f, SZ\_GPUPIC, SEEK\_CUR);

	gzread(f, psxM, 0x00200000);
	gzread(f, psxR, 0x00080000);
	gzread(f, psxH, 0x00010000);
	gzread(f, (void \*)&psxRegs, sizeof(psxRegs));

	if (Config.HLE)
		psxBiosFreeze(0);

	// gpu
	if (!gpufP)gpufP = (GPUFreeze\_t \*)malloc(sizeof(GPUFreeze\_t));
	gzread(f, gpufP, sizeof(GPUFreeze\_t));
	GPU\_freeze(0, gpufP);

	// spu
	gzread(f, &Size, 4);
	\_spufP = (SPUFreeze\_t \*)malloc(Size);
	gzread(f, \_spufP, Size);
	SPU\_freeze(0, \_spufP);
	free(\_spufP);

	sioFreeze(f, 0);
	cdrFreeze(f, 0);
	psxHwFreeze(f, 0);
	psxRcntFreeze(f, 0);
	mdecFreeze(f, 0);

	gzclose(f);

	return 0;
}
```

In which case we also need to move the read system ram and vram to the start of the function.

```c
gzread(f, psxM, 0x00200000);
...
// gpu
if (!gpufP)gpufP = (GPUFreeze\_t \*)malloc(sizeof(GPUFreeze\_t));
gzread(f, gpufP, sizeof(GPUFreeze\_t));
GPU\_freeze(0, gpufP);
```

That means the version checks for the state state version will come after, but I think that should be okay. So we might as well give this a shot and see if it works.
---
title: "Libreboard OSM"
description: "Installing Open Street Map on a 4GB memory SoC"
pubDate: "March 15, 2019"
heroImage: "/img/2_1_1024x1024-96834777.jpeg"
author: Kion
---

### Abstract

With the popularity of the Raspberry Pi, combined with the Raspberry Pi Foundations unwillingness to distribute more powerful hardware, there are a lot of board makers coming out with more and more powerful system on a chip computers to try and fill the void, and market, left by the Raspberry Pi. One of these makers is Libre Compters, and the Specific board I’m using is the ROC-RK3328-CC running Debian. Links for the hardware and Linux distribution can be found below.

Libre Computer: [https://libre.computer/products/boards/roc-rk3328-cc/](https://libre.computer/products/boards/roc-rk3328-cc/)  
Roc Documentation: [https://roc-rk3328-cc.readthedocs.io/en/latest/intro.html](https://roc-rk3328-cc.readthedocs.io/en/latest/intro.html)  
Debian: [http://download.t-firefly.com/Source/RK3328/ROC-RK3328-CC/Firmware/Debian/ROC-RK3328-CC\_Debian9-Arch64\_20180525.img.xz](http://download.t-firefly.com/Source/RK3328/ROC-RK3328-CC/Firmware/Debian/ROC-RK3328-CC_Debian9-Arch64_20180525.img.xz)

I personally got my hands on the 4GB version, and have the operating system installed on a 64GB SD card. I’d prefer to either have the eMMC card, but can’t seem to find a reasonably priced version from the stores available to me, or I’d like the option to boot from an external hard drive or SSD, but can’t find if this is supporter or not. Though specifically what I wanted to test is with more powerful ARM devices coming out, is it possible to install something like an Open Street Map tile server, to be able to serve regional maps from an SoC?

The source for this install is taken from [https://itsolutiondesign.wordpress.com/2017/07/11/build-your-own-openstreetmap-tile-server-on-ubuntu-16-04/](https://itsolutiondesign.wordpress.com/2017/07/11/build-your-own-openstreetmap-tile-server-on-ubuntu-16-04/), and has been copied to “show my work” on what specific commands are being executed in what order.

### Install PostGres

```bash
$ sudo apt install postgresql postgresql-contrib postgis postgresql-9.6-postgis-2.3 postgresql-9.6-postgis-scripts
```

Create Postgres Database

```bash
sudo -u postgres -i
createuser osm
createdb -E UTF8 -O osm gis
psql -c "CREATE EXTENSION hstore;" -d gis
psql -c "CREATE EXTENSION postgis;" -d gis
exit
```

Create OSM user and download map data

```bash
sudo adduser osm
su - osm
cd
wget https://github.com/gravitystorm/openstreetmap-carto/archive/v2.41.0.tar.gz
tar xvf v2.41.0.tar.gz
$ rm v2.41.0.tar.gz
$ wget -c http://download.geofabrik.de/asia/japan-latest.osm.pbf
exit
```

Create swap file (to prepare for import)

```bash
sudo dd if=/dev/zero of=/swapfile bs=1GB count=2
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Import map data

```bash
sudo apt-get install screen osm2pgsql
su - osm
osm2pgsql --slim -d gis -C 3600 --hstore -S openstreetmap-carto-2.41.0/openstreetmap-carto.style japan-latest.osm.pbf 
exit
```

And this is where my experiment came to an end. This process takes a long time even with more powerful systems, so I let the import run during the day. And when I got home at night, I found that the ROC-RK3328-CC was no longer available on the network. After rebooting the system, I would try to continue the process only to find that for some reason the SD card was now mounted as read-only, and I didn’t have any luck in mounting the SD card as read-write. I tried to reflash the SD card with Debian, and tried the process two more times and ended up with the same result.

I’m not sure if this is something done on my end, such as using a power supply that could not sustain the board for an extended period of time. I’m not sure if the board requires some manner of active cooling. I’m not sure if this is a bug in their compiled version of Debian, but at this point the process became frustrating enough that I would try their Ubuntu image option and only return to Debian if that proved successful.
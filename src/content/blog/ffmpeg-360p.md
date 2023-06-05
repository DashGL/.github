---
title: "ffmpeg convert to 360p webm"
description: "Code snippet for converting a video into 360p"
pubDate: "July 28, 2019"
heroImage: "/img/FFmpeg-Official-Logo-e1582927646246-1467730027.png"
author: Kion
---

[July 28, 2019](https://blog.dashgl.com/?p=566) [Kion](https://blog.dashgl.com/?author=1) [Uncategorized](https://blog.dashgl.com/?cat=1)

```bash
ffmpeg -i input.mp4 -filter:v “scale=-2:360” -vcodec libvpx -qmin 0 -qmax 50 -crf 10 -b:v 1M -acodec libvorbis output.webm
```

https://gist.github.com/dvlden/b9d923cb31775f92fa54eb8c39ccd5a9
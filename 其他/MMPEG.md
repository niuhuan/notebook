FFMPEG
======

## 1. 视频添加字幕

tips: ass文件名称含有空格时，提示过滤器无法识别。可能需要转义。

```shell
ffmpeg -i 'video.mkv' -vf 'subtitles=input.ass' -vcodec libx264 -acodec copy 'out.mp4'
```

默认是high10编码，在一些应用中无法播放, 需要加上`-profile:v main -pix_fmt yuv420p`参数

```shell
ffmpeg.exe -i 1.mkv  -vf 'subtitles=1.ass' -vcodec libx264 -profile:v main -pix_fmt yuv420p -acodec aac 1.mp4
```


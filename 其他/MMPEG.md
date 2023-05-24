FFMPEG
======

## 1. 视频添加字幕

tips: ass文件名称含有空格时，提示过滤器无法识别。可能需要转义。

```shell
ffmpeg -i 'video.mkv' -vf 'subtitles=input.ass' -vcodec libx264 -acodec copy 'out.mp4'
```


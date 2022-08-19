---
title: "ffmpeg"
weight: 1

date: 2022-08-17`T18:56:45+02:01
draft: false
---
# ffmpeg 

## See video metadata

The `ffprobe` gathers information from multimedia streams and prints it in human- and machine-readable fashion.
`ffprobe -i video.mkv`

## Showing the encoder options available

`ffmpeg -h encoder=hevc_nvenc`

## Converting a video to HVEC/H.265

It´s done by the `-c:v` parameter. To do it using the CPU:

`ffmpeg -i input.mov -map_metadata 0 -c:v libx265 -x265-params "lossless=1" output.mp4`

Using the GPU (Nvidia). Notice that in this codec we have a `-tune` parameter:
 
```
ffmpeg -h encoder=hevc_nvenc 
ffmpeg -i .input.mkv -map_metadata 0 -c:v hevc_nvenc -tune hq output.mp4
```

## Choosing the metadata, subtitles and audio

It´s done by the `-map` parameter.

Keeping all the video, audio and subtitle streams from input 1:

`ffmpeg.exe -i '.\sample.mkv' -map 0:v -map 0:a -map 0:s -map_metadata 0 -c:v hevc_nvenc -tune hq filme.mkv`

Keeping the video, but only audio 2 and subtitle 3 from input 1. Notice that it is indexed by 0:

`ffmpeg.exe -i '.\sample.mkv' -map 0:v -map 0:a:1 -map 0:s:2 -map_metadata 0 -c:v hevc_nvenc -tune hq filme.mkv`

The 1st video stream, 2nd audio stream, all subtitles: 

`ffmpeg -i input -map 0:v:0 -map 0:a:1 -map 0:s -c copy output`

The 3rd video stream, all audio streams, no subtitles. This example uses negative mapping to exclude the subtitles:

`ffmpeg -i input -map 0:v:2 -map 0:a -map -0:s -c copy output`

Choosing streams from multiple inputs. All video from input 0, all audio from input 1:

`ffmpeg -i input0 -i input1 -map 0:v -map 1:a -c copy output`


## Links:

 - https://ntown.at/de/knowledgebase/cuda-gpu-accelerated-h264-h265-hevc-video-encoding-with-ffmpeg/
 - https://trac.ffmpeg.org/wiki/Encode/H.265
 - https://ieeexplore.ieee.org/document/7946796

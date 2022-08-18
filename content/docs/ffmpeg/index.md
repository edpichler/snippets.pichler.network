---
title: "ffmpeg"

date: 2022-08-17`T18:56:45+02:01
draft: true
---
# ffmpeg 

## Converting a video to HVEC/H.265:
Using the CPU:
`ffmpeg -i input.mov -map_metadata 0 -c:v libx265 -x265-params "lossless=1" output.mp4`

Using the GPU (Nvidia):
 `ffmpeg -i .input.mkv -map_metadata 0 -c:v hevc_nvenc -x265-params "lossless=1" output.mp4`

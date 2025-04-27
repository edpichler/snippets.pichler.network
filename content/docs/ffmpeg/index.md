---
title: "ffmpeg"
weight: 8

date: 2022-08-17`T18:56:45+02:01
draft: false
---
# ffmpeg

## List encoders available in ffmpeg:
- `ffmpeg -encoders`
- `ffmpeg -encoders | grep nvenc`
- `ffmpeg -h encoder=hevc_nvenc` to know more about the options available for the encoder.

## See video metadata

The `ffprobe` gathers information from multimedia streams and prints it in human- and machine-readable fashion.
`ffprobe -i video.mkv`

## Converting a video to HVEC/H.265

It's done by the `-c:v` parameter:

`ffmpeg -i input.mov -map_metadata 0 -c:v libx265 -x265-params "lossless=1" output.mp4`

If you want to use the GPU (Nvidia), do `-c:v hevc_nvenc` instead. Doing `ffmpeg -h encoder=hevc_nvenc` you can know all the options available for the encoder.

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

## Trimming a video

Without re-encoding:

`ffmpeg -ss 00:45:00 -i huge_video.mp4 -to 00:05:00 -c copy clipped.mp4`

## Running ffmpeg using different combinations of parameters

```powershell
$filter = "*209_video.mp4"
$files = Get-ChildItem -Filter $filter

$nvencPresets = @("p1", "p4", "p7")
$nvecTunes = @("hq", "uhq", "ll", "ull")
$targetQualityLevel = @("0", "10", "15", "20", "25", "30", "35", "40", "45", "50")

$variableBitrate = "-rc vbr -b:v 0"
$pixFormat = "-pix_fmt yuv444p"
$mapVideoAudioSubs = "-map 0:v" #-map 0:a -map 0:s"
$mapFileMetadata = "-map_metadata 0 -movflags use_metadata_tags"
$from = "-ss 7"
$to = "-t 5"
$scales = @("350", "480", "720", "1080", "1440", "2160", "4320")

foreach ($preset in $nvencPresets) {
  foreach ($tune in $nvecTunes) {
    foreach ($targetQuality in $targetQualityLevel) {
      foreach ($scale in $scales) {
        $scaleArg = "-vf scale=-1:" + $scale
        foreach ($file in $files) {
            $theCommand = "ffmpeg $from -i $file $to $mapFileMetadata $mapVideoAudioSubs "
            $newname = $file.Name.Remove($file.Name.Length - $file.Extension.Length)
            $newname += "." + $scale + "p.ffmpeg.nvenc.cq_$targetQuality.tune_$tune.preset_$preset.mkv"
            $theCommand += " -c:v hevc_nvenc $scaleArg -tune $tune -cq $targetQuality $variableBitrate $pixFormat -preset $preset $newname"
            $currentDateTime = (Get-Date).ToString("yyyy-MM-dd HH:mm:ss")
            $currentDateTime + "; " + $theCommand | Out-File -FilePath "$($file)_ffmpeg_commands_executed.log" -Encoding utf8 -Append
            Invoke-Expression $theCommand
        }
      }
    }
  }
}

```

Again, although, multiple scales arguments in the same command:

```powershell
$filter = "*209_video.mp4"
$files = Get-ChildItem -Filter $filter

$nvencPresets = @("p1", "p7")
$nvecTunes = @("hq", "uhq", )
$targetQualityLevel = @("0", "10", "15", "20", "25", "30", "35", "40", "45", "50")
$scales = @("350", "4320")

$variableBitrate = "-rc vbr -b:v 0"
$pixFormat = "-pix_fmt yuv444p"
$mapVideoAudioSubs = "-map 0:v" #-map 0:a -map 0:s"
$mapFileMetadata = "-map_metadata 0 -movflags use_metadata_tags"
$from = "-ss 7"
$to = "-t 5"

foreach ($file in $files) {
  foreach ($preset in $nvencPresets) {
    foreach ($tune in $nvecTunes) {
      foreach ($targetQuality in $targetQualityLevel) {
        $theCommand = "ffmpeg $from -i $file $to $mapFileMetadata $mapVideoAudioSubs "
        foreach ($scale in $scales) {
          $scaleArg = "-vf scale=-1:" + $scale
          $newname = $file.Name.Remove($file.Name.Length - $file.Extension.Length)
          $newname += "."+ $scale + "p.ffmpeg.nvenc.cq_$targetQuality.tune_$tune.preset_$preset.mkv"
          $theCommand += " -c:v hevc_nvenc $scaleArg -tune $tune -cq $targetQuality $variableBitrate $pixFormat -preset $preset $newname "
        }

        $currentDateTime = (Get-Date).ToString("yyyy-MM-dd HH:mm:ss.fff")
        $currentDateTime + "; " + $theCommand | Out-File -FilePath "$($file)_ffmpeg_commands_executed.log" -Encoding utf8 -Append
        Invoke-Expression $theCommand
      }
    }
  }
}


```
## Links:

 - https://ntown.at/de/knowledgebase/cuda-gpu-accelerated-h264-h265-hevc-video-encoding-with-ffmpeg/
 - https://trac.ffmpeg.org/wiki/Encode/H.265
 - https://ieeexplore.ieee.org/document/7946796

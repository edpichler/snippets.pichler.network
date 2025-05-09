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
$filter = "*_video.mp4"
$files = Get-ChildItem -Filter $filter

$nvencPresets = @("p1", "p4", "p7")
$nvecTunes = @("hq", "uhq", "ll", "ull")
$targetQualityLevel = @("0", "10", "15", "20", "25", "30", "35", "40", "45", "50")

$variableBitrate = "-rc vbr -b:v 0"
$pixFormat = "-pix_fmt yuv444p"
$mapVideoAudioSubs = "-map 0:v" #-map 0:a -map 0:s"
$mapFileMetadata = "-map_metadata 0 -movflags use_metadata_tags"
$from = "-ss 7"
$duration = "-t 5"
$scales = @("350", "480", "720", "1080", "1440", "2160", "4320")

foreach ($preset in $nvencPresets) {
  foreach ($tune in $nvecTunes) {
    foreach ($targetQuality in $targetQualityLevel) {
      foreach ($scale in $scales) {
        $scaleArg = "-vf scale=-1:" + $scale
        foreach ($file in $files) {
            $theCommand = "ffmpeg $from -i $file $duration $mapFileMetadata $mapVideoAudioSubs "
            $newname = $file.Name.Remove($file.Name.Length - $file.Extension.Length)
            $newname += "." + $scale + "p.ffmpeg.nvenc.cq_$targetQuality.tune_$tune.preset_$preset.mkv"
            $theCommand += " -c:v hevc_nvenc $scaleArg -tune $tune -cq $targetQuality $variableBitrate $pixFormat -preset $preset $newname"
            $currentDateTime = (Get-Date).ToString("yyyy-MM-dd HH:mm:ss")
            $currentDateTime + "; " + $theCommand | Out-File -FilePath "$($file)_ffmpeg_commands_executed.log" -Encoding utf8 -Append
            Invoke-Expression $theCommand

            # comparision video using images: optional
            $outputDirectory = "comparison_images"
            $scaleHeight = 1080
            # Create a directory to store the comparison images
            if (-not (Test-Path $outputDirectory)) {
                New-Item -Path $outputDirectory -ItemType Directory
            }

            $frame1 = "$outputDirectory/frame1.png"
            $frame2 = "$outputDirectory/frame2.png"
            $output = "$outputDirectory/comparison_$newname.jpg"
            $extractFrameCmd = "ffmpeg $from -i $file -vframes 1 -q:v 1 $frame1"
            Invoke-Expression $extractFrameCmd
            $extractFrameCmd = "ffmpeg -i $newname -vframes 1 -q:v 1 $frame2"
            Invoke-Expression $extractFrameCmd

            $label1 = "Original: $file"
            $bitrate = $(ffprobe -v error -show_entries format=bit_rate -of default=noprint_wrappers=1:nokey=1  $newname)
            $label2 = "Compressed. Preset: $preset; Tune: $tune; CQ: $targetQuality; Scale: $scale; Bitrate: $bitrate;"
            magick mogrify -gravity northwest -pointsize 20 -fill black -annotate +18+18 $label1 -fill white -annotate +16+16 $label1 $frame1
            magick mogrify -gravity northwest -pointsize 20 -fill black -annotate +18+18 $label2 -fill white -annotate +16+16 $label2 $frame2

            ffmpeg -i $frame1 -i $frame2 -filter_complex "[0]scale=-1:$scaleHeight[frame1]; [1]scale=-1:$scaleHeight[frame2]; [frame1][frame2]hstack=inputs=2" -q:v 1 $output

            Remove-Item $frame1, $frame2

        }
      }
    }
  }
}

```

## Compare videos side by side

```powershell
$video1 = "original.mp4"
$video2 = "otherVideo.mkv"
$output = "comparision_video1_video2.mp4"
$duration = 10

# Create comparision video
ffmpeg -i $video1 -i $video2 -filter_complex "[0:v]scale=-1:2160[vid1]; [1:v]scale=-1:2160[vid2]; [vid1][vid2]hstack=inputs=2" -c:v hevc_nvenc -preset p1 -tune lossless -t $duration $output

```

## Compare videos side by side using images

``` powershell

$video1 = "video1.mp4"
$video2 = "video2.mp4"

$outputDirectory = "comparison_images"
$interval = 10
$scaleHeight = 1080
$duration = $(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 $video1)

# Create a directory to store the comparison images
if (-not (Test-Path $outputDirectory)) {
    New-Item -Path $outputDirectory -ItemType Directory
}

for ($timestamp = 0; $timestamp -lt $duration; $timestamp += $interval) {
    $frame1 = "$outputDirectory/frame$timestamp-a.png"
    $frame2 = "$outputDirectory/frame$timestamp-b.png"
    $frameCombined = "$outputDirectory/frame$timestamp-combined.png"
    ffmpeg -ss $timestamp  -i $video1 -vframes 1 -q:v 1 $frame1
    ffmpeg -ss $timestamp  -i $video2 -vframes 1 -q:v 1 $frame2
    $label1 = "Original: $video1"
    $label2 = "Compressed: $video2"
    magick mogrify -gravity northwest -pointsize 20 -fill black -annotate +18+18 $label1 -fill white -annotate +16+16 $label1 $frame1
    magick mogrify -gravity northwest -pointsize 20 -fill black -annotate +18+18 $label2 -fill white -annotate +16+16 $label2 $frame2
    ffmpeg -i $frame1 -i $frame2 -filter_complex "[0]scale=-1:$scaleHeight[frame1]; [1]scale=-1:$scaleHeight[frame2]; [frame1][frame2]hstack=inputs=2" -q:v 1 $frameCombined
}
```

## Links:

 - https://ntown.at/de/knowledgebase/cuda-gpu-accelerated-h264-h265-hevc-video-encoding-with-ffmpeg/
 - https://trac.ffmpeg.org/wiki/Encode/H.265
 - https://ieeexplore.ieee.org/document/7946796

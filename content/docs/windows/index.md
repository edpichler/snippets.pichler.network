---
title: "Windows"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## Run a command recursivelly using the PowerShell:

```
dir *.* | foreach-object { $newname = $_.Name.Remove($_.Name.Length - $_.Extension.Length) + ".mp4"; .\ffmpeg.exe -i "$_"  $newname }

```  
Some properties in the returning variable are: $_.Name, $_.BaseName, $_.FullName.

## Creating hardlinks in Windows

``` bash
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

## Restart the Ubuntu subsystem without restarting the Windows

```
#see what is running
wsl -l -v

#shutdown everything
wsl --shutdown 

#start the distro
wsl -d <DistroName>
```
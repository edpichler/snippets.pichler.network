---
title: "Windows"
weight: 1
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

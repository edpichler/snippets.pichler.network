---
title: "ImageMagick"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## Reducing the image width size

**Waring:** Notice that mogrify replaces the original file

`magick mogrify -resize 600x^> *.jpg`

## Creating thumbnails (Windows)

$filter = "*.cr3"
$newExtension = "_thumbnail.jpeg"
$files = Get-ChildItem -Filter $filter
$totalFiles = $files.Count
$currentFile = 0

foreach ($file in $files) {
    $currentFile++
    $newname = $file.Name.Remove($file.Name.Length - $file.Extension.Length) + $newExtension
    magick "$file" -auto-orient -thumbnail 1000x1000 $newname
    echo "Generating thumbnail for $file to $newname ($currentFile of $totalFiles)"
}
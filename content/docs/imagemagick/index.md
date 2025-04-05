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

foreach ($file in $files) {
    $newname = $file.Name.Remove($file.Name.Length - $file.Extension.Length) + $newExtension
    ./magick.exe -thumbnail "$file" 1000x1000 $newname
    echo "Thumbnail generated for $file in $newname"
}
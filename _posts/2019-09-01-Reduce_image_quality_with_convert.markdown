---
layout: post
title:  "Reduce image quality with 'convert'"
date:   2019-09-01 07:04:16 -0300
tags: computers image imagemagick linux 
---
I'm sure that thumbnails doesn't need to be 1MB.

`convert` is a command which is part of [ImageMagick](http://www.imagemagick.org) and it is a swiss knife of image manipulation. Here I just want to show one very common (and simple use case): reducing the quality of an image.

```bash
$ convert -strip -interlace Plane -gaussian-blur 0.05 -quality 85% ORIGINAL.jpg CONVERTED.jpg
```

#### Parameters
* `-strip`: strip image of all profiles and comments
* `-interlace Plane`: type of image interlacing scheme
* `-gaussian-blur 0.05`: reduce image noise and reduce detail levels
* `-quality 85%`: JPEG/MIFF/PNG compression level (less means less quality)


That's it!

#### Tip
If you want to apply the changes on the original file, just the filename twice:
```bash
$ convert -strip -interlace Plane -gaussian-blur 0.05 -quality 85% ORIGINAL.jpg ORIGINAL.jpg
```

### Keep reading
* [Complete `convert` guide](https://imagemagick.org/script/convert.php) 


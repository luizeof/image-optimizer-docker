# Image Optimizer

Optimize all PNG, JPG images from directories.

## Pull image

```bash
docker pull luizeof/image-optimizer
```

## Source directory

This container creates a volume at `/source`, so you can mount a local directory to this remote directory and access any file within.

***If OptiPNG can optimize the input file it will be overwritten with the optimized one.***

## Run
```
$ docker run luizeof/image-optimzer -help

OptiPNG 0.6.4: Advanced PNG optimizer.
Copyright (C) 2001-2010 Cosmin Truta.

Synopsis:
    optipng [options] files ...
Files:
    Image files of type: PNG, BMP, GIF, PNM or TIFF
Basic options:
    -?, -h, -help       show this help
    -o <level>          optimization level (0-7)                default 2
    -v                  verbose mode / show copyright and version info
General options:
    -fix                enable error recovery
    -force              enforce writing of a new output file
    -keep               keep a backup of the modified files
    -preserve           preserve file attributes if possible
    -quiet              quiet mode
    -simulate           simulation mode
    -snip               cut one image out of multi-image or animation files
    -out <file>         write output file to <file>
    -dir <directory>    write output file(s) to <directory>
    -log <file>         log messages to <file>
    --                  stop option switch parsing
Optimization options:
    -f  <filters>       PNG delta filters (0-5)                 default 0,5
    -i  <type>          PNG interlace type (0-1)                default <input>
    -zc <levels>        zlib compression levels (1-9)           default 9
    -zm <levels>        zlib memory levels (1-9)                default 8
    -zs <strategies>    zlib compression strategies (0-3)       default 0-3
    -zw <window size>   zlib window size (32k,16k,8k,4k,2k,1k,512,256)
    -full               produce a full report on IDAT (might reduce speed)
    -nb                 no bit depth reduction
    -nc                 no color type reduction
    -np                 no palette reduction
    -nx                 no reductions
    -nz                 no IDAT recoding
Optimization details:
    The optimization level presets
        -o0  <=>  -o1 -nx -nz
        -o1  <=>  [use the libpng heuristics]   (1 trial)
        -o2  <=>  -zc9 -zm8 -zs0-3 -f0,5        (8 trials)
        -o3  <=>  -zc9 -zm8-9 -zs0-3 -f0,5      (16 trials)
        -o4  <=>  -zc9 -zm8 -zs0-3 -f0-5        (24 trials)
        -o5  <=>  -zc9 -zm8-9 -zs0-3 -f0-5      (48 trials)
        -o6  <=>  -zc1-9 -zm8 -zs0-3 -f0-5      (120 trials)
        -o7  <=>  -zc1-9 -zm8-9 -zs0-3 -f0-5    (240 trials)
    The libpng heuristics
        -o1  <=>  -zc9 -zm8 -zs0 -f0            (if PLTE is present)
        -o1  <=>  -zc9 -zm8 -zs1 -f5            (if PLTE is not present)
    The most exhaustive search (not generally recommended)
      [no preset] -zc1-9 -zm1-9 -zs0-3 -f0-5    (1080 trials)
Examples:
    optipng file.png                            (default speed)
    optipng -o5 file.png                        (moderately slow)
    optipng -o7 file.png                        (very slow)
    optipng -i1 -o7 -v -full -sim experiment.png
``` 

### Optimize all images in current folder
By default this image will compress with `-quiet -o7 *.png` and as such will optimize all images with the best optimization level.

```
$ docker run -v /local-path-to-image:/source luizeof/image-optimizer -quiet -o7 *.png
```

### Recursivley optimize all images
The following command finds all images in the current folder (`find...`), mounts it to the Docker volume (`-v ...`) and optimizes the image with the best optimization level (`-o7`).
```
$ find . -name "*.png" | xargs docker run -t -v `pwd`:/source luizeof/image-optimizer -o7
```

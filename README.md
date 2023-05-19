# xournaltomp4

Turn files created by [xournalpp](https://xournalpp.github.io/) (with a `xopp` extension)
into `mp4` videos.

[<img src="sample1.mp4" width="50%">](https://github.com/kacpertopol/xournaltomp4/blob/main/sample1.mp4)

# requirements

In order to work the script requires:

- `python3`
- `zcat`
- `gzip`
- `ffmpeg`
- `xournalpp`

# usage

Basic usage is for example:

```
$ xopptomp4 file.xopp videoname
```

This will create a directory `videoname` that contains frames for the video 
(and log files `out`, `err`)
and a file `videoname.mp4` that contains the video.

# options

The default options should result in a decent looking video but there
are some parameters that can / should be tweaked.

## horizontal resolution

Lowering the horizontal resolution 

```
$ xopptomp4 --hres 640 file.xopp videoname
```

is usefull for creating a quick preview of the video.

## number of threads

By default `xopptomp4` uses 4 threads. This can be changed, for example:

```
$ xopptomp4 --nthreads 2 file.xopp videoname
```
will result in 2 threads being ran.

## other

Help information is available by running:

```
$ xopptomp4 --help
```

or 

```
$ xopptomp4 -h
```

This provides a list of options with descriptions and default values:

```
usage: xopptomp4 [-h] [--hres HRES] [--frate FRATE] [--pause PAUSE] [--images IMAGES] [--fchange] [--nthreads NTHREADS]
                 [--endpause ENDPAUSE] [--layerpause LAYERPAUSE] [--skipevery SKIPEVERY]
                 input output

Turn xojp file into a mpeg.

positional arguments:
  input                 Input file.
  output                Output directory.

options:
  -h, --help            show this help message and exit
  --hres HRES, -r HRES  Horizontal resulution, by default 1920.
  --frate FRATE, -f FRATE
                        Frame rate, by default 32.
  --pause PAUSE, -p PAUSE
                        Frames to pause on short strokes. By default 4.
  --images IMAGES, -i IMAGES
                        Frames to pause on images. By default 32.
  --fchange, -c         Change framerate, does not calculate frames.
  --nthreads NTHREADS, -t NTHREADS
                        Number of threads to run in parallel. By default 4.
  --endpause ENDPAUSE, -e ENDPAUSE
                        Frames to pause on end of page. By default 64.
  --layerpause LAYERPAUSE, -l LAYERPAUSE
                        Frames to pause on end of layer. By default 32.
  --skipevery SKIPEVERY, -s SKIPEVERY
                        Take one stroke coordinate every copuple of coordinates for frames. By default 8.
```



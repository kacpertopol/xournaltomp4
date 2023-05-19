# xournaltomp4

Turn files created by [xournalpp](https://xournalpp.github.io/) (with a `xopp` extension)
into `mp4` videos.

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

This provides a list of options with descriptions and default values.



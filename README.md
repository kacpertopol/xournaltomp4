# xournaltomp4

Turn files created by [xournalpp](https://xournalpp.github.io/) (with a `xopp` extension)
into `mp4` videos.

https://github.com/kacpertopol/xournaltomp4/assets/9459444/c54b93e5-ffc2-4dca-bd3a-d44d794547cf

# requirements

In order to work the script requires:

- `python3`
- `zcat`
- `gzip`
- `ffmpeg`
- `xournalpp`

Only strokes with varrying width written using a pressure sensitive stylus will be animated.
Other objects will display all at once (there was only a limited amount of testing so please report problems).

# usage

Basic usage is for example:

```
$ xopptomp4 file.xopp videoname
```

This will create a directory `videoname` that contains frames (`png` files by default) for the video  
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
                 [--endpause ENDPAUSE] [--layerpause LAYERPAUSE] [--skipevery SKIPEVERY] [--animate] [--marked] [--svg] [--printout]
                 [--wait WAIT]
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
  --animate, -a         Use different method for splitting presentation. Layers are not placed on top of one another but animated.
                        One layer at a time, the first layer appears on all frames.
  --marked, -m          Use different method for splitting presentation. Layers are not placed on top of one another but animated.
                        Layers marked with "#SKIP#" will not show when further layers are drawn.
  --svg, -v             Save frames in svg format instead of png.
  --printout, -o        Make printout.
  --wait WAIT, -w WAIT  Path to file with wait times for pages. Each line has the following format: <page number starting from 1>
                        <hours>:<minutes>:<seconds>:<hundredths of a second>. Page numbering is consistent with xournal's --export-
                        layers-progressively. Lines containig # will be ignored.
```

# synchronizing with sound

This is extremely experimental right now. The suggested workflow for synchronizing with sound is:

1) create a `xournalpp` document for example `document.xopp`
2) export it to `pdf` using the `--export-layers-progressively` option:
   `$ xournalpp --export-layers-progressively --create-pdf=document.pdf document.xopp`. 
3) this will create `document.pdf` with ... 
   the layers exported progressively ... The page numbers in this document can be used
   to time the video
4) record the audion while looking at `document.pdf`
5) listen to the audio and make a note down specific timestamps in a file, for example `timestamps`, with the format:
```
# this is the time <hours>:<minutes>:<seconds>:<hundredths of a second>
# that the audio for the first page of document.pdf ends
1 00:00:09:12
# this is the time <hours>:<minutes>:<seconds>:<hundredths of a second>
# that the audio for the second page of document.pdf ends
2 00:00:15:02
# time the audion for the third page ends, ...
3 00:00:20:19
# this is a comment btw
```
6) run `xopptomp4` with the option `--wait timestamps` to use the time data in creating the video
7) use `ffmpeg` (or other software) to join the video and audio, they should be synchronized nicely

# tips

The script takes a long time to render a video, even with multiple threads.
In some case a good approach might be to break up the `xopp` document into 3-4 page parts,
turning each part in to separate videos and then concatenating them.

Using the `--svg` option may decrease the time for frame calculating but
the size of the directory with the frames will be larger. This
is a bit counterintuative - `svg` is a vector fromat while `png` is a bitmap format -
however the lossless compression for the `png` files workes very well in this case.
After compressing both directories (eg using `zip`) the size of the archive with the
`svg` files might be smaller. Using `svg` files might be beneficial for upscaling the
video resolution.

The script looks for strokes made using a pressure sensitive stylus and animates them.
If lines and curves are created with constant width, they will not be animated currently.
Animating strokes with constant width might be something to add in the future.

# IMPORTANT

This is a very short simple script. It might not work perfectly, please let me know of any issues using github's issue system. 

Please read the `LICENSE`.

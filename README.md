# Corozo
A simple script to encode 4K HDR content using ffmpeg

## Blog post explanation
For a more in-depth explanation of this script and its options, [please visit this post](https://medium.com/@yllanos/how-to-encode-a-4k-hdr-movie-using-ffmpeg-while-maintaining-selected-auio-tracks-intact-from-source-d1e2f6a16162)

## Requirements
ffmpeg 3.x or later with libx265 configured. For more detail, Iḿ going to share my config (as enabled by my Linux distribution):

```Shell

configuration: --prefix=/usr --extra-version='1~deb9u1' --toolchain=hardened \
--libdir=/usr/lib/x86_64-linux-gnu --incdir=/usr/include/x86_64-linux-gnu \
--enable-gpl --disable-stripping --enable-avresample --enable-avisynth --enable-gnutls \
--enable-ladspa --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca \
--enable-libcdio --enable-libebur128 --enable-libflite --enable-libfontconfig \
--enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm \
--enable-libmp3lame --enable-libopenjpeg --enable-libopenmpt --enable-libopus \
--enable-libpulse --enable-librubberband --enable-libshine --enable-libsnappy \
--enable-libsoxr --enable-libspeex --enable-libssh --enable-libtheora \
--enable-libtwolame --enable-libvorbis --enable-libvpx --enable-libwavpack \
--enable-libwebp --enable-libx265 --enable-libxvid --enable-libzmq \
--enable-libzvbi --enable-omx --enable-openal --enable-opengl --enable-sdl2 \
--enable-libdc1394 --enable-libiec61883 --enable-chromaprint --enable-frei0r \
--enable-libopencv --enable-libx264 --enable-shared

```

## The script
```Shell

nohup ffmpeg -hide_banner \
-i "../storage/My.Source.Movie.2019.2160p.BluRay.REMUX.HEVC.DTS-HD.MA.TrueHD.7.1.Atmos.mkv" \
-pix_fmt yuv420p10le \
-map_chapters 0 \
-metadata title="My Awesome Movie (2019)" \
-map 0:0 -metadata:s:v:0 language=eng -metadata:s:v:0 title="My Awesome Movie (2019)" \
-map 0:1 -metadata:s:a:0 language=eng -metadata:s:a:0 title="Dolby TrueHD 7.1 (Atmos)" \
-map 0:7 -metadata:s:a:0 language=spa -metadata:s:a:1 title="EAC-3 7.1" \
-map 0:2 -metadata:s:a:0 language=eng -metadata:s:a:2 title="DTS-HD MA 7.1" \
-map 0:4 -metadata:s:a:0 language=eng -metadata:s:a:3 title="AC-3 2.0" \
-metadata:s:t:0 filename="" -metadata:s:t:0 mimetype="image/jpeg" \
-c:v libx265 -preset fast -crf 21 \
-x265-params keyint=60:bframes=3:vbv-bufsize=75000:vbv-maxrate=75000:hdr-opt=1:repeat-headers=1:colorprim=bt2020:transfer=smpte-st-2084:colormatrix=bt2020nc:master-display="G(13250,34500)B(7500,3000)R(34000,16000)WP(15635,16450)L(10000000,500)" \
-c:a copy \
My.Movie.2019.2160p.BluRay.REMUX.HEVC.DTS-HD.MA.TrueHD.7.1.Atmos.mkv  > job.log &


```

## To check job progress
```Shell

tail -f job.log

```

## Things that could be improved

* Receive a paramater for input file name and/or location
* Receive a parameter for output file name and/or location
* Receive a parameter for log file name and/or location
* Show a progress bar (I understand ffmpeg does not have this option)
* Provide a menu to interactively ask for parameters
* Detect existing files based on names provided and ask for deletion confirmation or renaming


If you are willing to contribute, please create a branch, work on your own and reach out to me via pull request. I can have a look and collaborate.


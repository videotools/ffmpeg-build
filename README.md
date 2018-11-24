FFmpeg Build Docker image
==========================

 [![Docker Stars](https://img.shields.io/docker/stars/video-tools/ffmpeg-build.svg?style=plastic)](https://registry.hub.docker.com/v2/repositories/video-tools/ffmpeg-build/stars/count/) [![Docker pulls](https://img.shields.io/docker/pulls/video-tools/ffmpeg-build.svg?style=plastic)](https://registry.hub.docker.com/v2/repositories/video-tools/ffmpeg-build/)
[![Travis](https://img.shields.io/travis/video-tools/ffmpeg-build/master.svg?maxAge=300?style=plastic)](https://travis-ci.org/video-tools/ffmpeg-build)
[![Build Status](https://dev.azure.com/video-tools/ffmpeg-build/_apis/build/status/jrottenberg.ffmpeg)](https://dev.azure.com/video-tools/ffmpeg-build/_build/latest?definitionId=1)
[![Docker Automated build](https://img.shields.io/docker/automated/video-tools/ffmpeg-build.svg?maxAge=2592000?style=plastic)](https://github.com/video-tools/ffmpeg-build/)

This project prepares a development Docker image for FFmpeg. It compiles FFmpeg from sources following instructions from the [Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide). All the generated libs are left in the final image to be reuse.

You can test the latest build of this image by running `docker pull video-tools/ffmpeg-build`.

This image can be used as a base for an encoding farm.

Ubuntu builds
--------------

You can use video-tools/ffmpeg-build to get the latest build based on ubuntu.



<details><summary>(How the 'recent images' was generated)</summary>
```
    $ curl --silent https://hub.docker.com/v2/repositories/video-tools/ffmpeg-build/tags/?page_size=500 | jq -cr ".results|sort_by(.name)|reverse[]|.sz=(.full_size/1048576|floor|tostring+\"mb\")|[.name,( (20-(.name|length))*\" \" ),.sz,( (8-(.sz|length))*\" \"),.last_updated[:10]]|@text|gsub(\"[,\\\"\\\]\\\[]\";null)" | grep 2018-08
```
</details>

Please use [Github issues](https://github.com/video-tools/ffmpeg-build/issues/new) to report any bug or missing feature.



See what's inside the beast
---------------------------

```
docker run -it --entrypoint='bash' video-tools/ffmpeg-build

for i in ogg amr vorbis theora mp3lame opus vpx xvid fdk x264 x265;do echo $i; find /usr/local/ -name *$i*;done
```

Keep up to date
---------------

See Dockerfile-env to update a version

- [FFMPEG_VERSION](http://ffmpeg.org/releases/): [GNU Lesser General Public License (LGPL) version 2.1](https://ffmpeg.org/legal.html)
- [OGG_VERSION](https://xiph.org/downloads/): [BSD-style license](https://git.xiph.org/?p=mirrors/ogg.git;a=blob_plain;f=COPYING;hb=HEAD)
- [OPENCOREAMR_VERSION](https://sourceforge.net/projects/opencore-amr/files/opencore-amr/): [Apache License](https://sourceforge.net/p/opencore-amr/code/ci/master/tree/LICENSE)
- [VORBIS_VERSION](https://xiph.org/downloads/): [BSD-style license](https://git.xiph.org/?p=mirrors/vorbis.git;a=blob_plain;f=COPYING;hb=HEAD)
- [THEORA_VERSION](https://xiph.org/downloads/): [BSD-style license](https://git.xiph.org/?p=mirrors/theora.git;a=blob_plain;f=COPYING;hb=HEAD)
- [LAME_VERSION](http://lame.sourceforge.net/download.php): [GNU Lesser General Public License (LGPL) version 2.1](http://lame.cvs.sourceforge.net/viewvc/lame/lame/LICENSE?revision=1.9)
- [OPUS_VERSION](https://www.opus-codec.org/downloads/): [BSD-style license](https://www.opus-codec.org/license/)
- [VPX_VERSION](https://github.com/webmproject/libvpx/releases): [BSD-style license](https://github.com/webmproject/libvpx/blob/master/LICENSE)
- [XVID_VERSION](https://labs.xvid.com/source/): [GNU General Public Licence (GPL) version 2](http://websvn.xvid.org/cvs/viewvc.cgi/trunk/xvidcore/LICENSE?revision=851)
- [FDKAAC_VERSION](https://github.com/mstorsjo/fdk-aac/releases): [Liberal but not a license of patented technologies](https://github.com/mstorsjo/fdk-aac/blob/master/NOTICE)
- [FREETYPE_VERSION](http://download.savannah.gnu.org/releases/freetype/): [GNU General Public License (GPL) version 2](https://www.freetype.org/license.html)
- [LIBVIDSTAB_VERSION](https://github.com/georgmartius/vid.stab/releases): [GNU General Public License (GPL) version 2](https://github.com/georgmartius/vid.stab/blob/master/LICENSE)
- [LIBFRIDIBI_VERSION](https://www.fribidi.org/): [GNU General Public License (GPL) version 2](https://cgit.freedesktop.org/fribidi/fribidi/plain/COPYING)
- [X264_VERSION](http://www.videolan.org/developers/x264.html): [GNU General Public License (GPL) version 2](https://git.videolan.org/?p=x264.git;a=blob_plain;f=COPYING;hb=HEAD)
- [X265_VERSION](https://bitbucket.org/multicoreware/x265/downloads/):[GNU General Public License (GPL) version 2](https://bitbucket.org/multicoreware/x265/raw/f8ae7afc1f61ed0db3b2f23f5d581706fe6ed677/COPYING)


Contribute
-----------


```
# Add / fix stuff
${EDITOR} templates/

# Generates the Dockerfile for all variants
./update.py

# Test a specific variant
docker build -t my-build docker-images/VERSION/

# Make sure all variants pass before Travis does
find ffmpeg/ -name Dockerfile | xargs dirname | parallel --no-notice -j 4 --results logs docker build -t {} {}
```


Commit the templates files THEN all the generated Dockerfile for a merge request. So it's easier to review the template change.

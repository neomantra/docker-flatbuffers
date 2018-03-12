# docker-flatbuffers - Docker tooling for Flatbuffers

[![Build Status](https://travis-ci.org/neomantra/docker-flatbuffers.svg?branch=master)](https://travis-ci.org/neomantra/docker-flatbuffers)  [![](https://images.microbadger.com/badges/image/neomantra/flatbuffers.svg)](https://microbadger.com/#/images/neomantra/flatbuffers "microbadger.com")

## Supported tags

Images are availble in both `gcc` and `clang` variants, representing which toolchain was FlatBuffers was built from.  The plain, undecorated tag name uses `gcc`.   "master" branches are tagged with YYYYMMDD of the image build dates.  Specific FlatBuffer releases are tagged with their version name.

- neomantra/flatbuffers, neomantra/flatbuffers:gcc
- neomantra/flatbuffers:clang
- neomantra/flatbuffers:v1.8.0, neomantra/flatbuffers:gcc-v1.8.0
- neomantra/flatbuffers:clang-v1.8.0

The builds are managed by [Travis-CI](https://travis-ci.org/neomantra/docker-openresty).

For best stability, pin your images to the full tag, for example `neomantra/flatbuffers:gcc-v1.8.0`.


Table of Contents
=================

* [Description](#description)
* [Usage As An Image Build Stage](#usage-as-an-image-build-stage)
* [Usage As A Build Tool](#usage-as-a-build-tool)
* [Files In This Image](#files-in-this-image)
* [Customizing The Docker Image Build](#customizing-the-docker-image-build)
* [Authors](#authors)
* [Copyright & License](#copyright--license)


Description
===========

`docker-flatbuffers` is [Docker](https://www.docker.com) tooling for [FlatBuffers](https://google.github.io/flatbuffers/).

The builds are managed by [Travis-CI](https://travis-ci.org/neomantra/docker-flatbuffers).


Usage As An Image Build Stage
=============================

You can `COPY` from this image into your own images using the following Dockerfile stanzas.  Select the image, tag, and artifacts most appropriate for your project.   The included files are listed in the [Files In This Image](#files-in-this-image) section.

```
COPY --from=neomantra/flatbuffers /usr/local/bin/flatc /usr/local/bin/flatc
COPY --from=neomantra/flatbuffers /usr/local/include/flatbuffers /usr/local/include/flatbuffers
COPY --from=neomantra/flatbuffers /usr/local/lib/libflatbuffers.a /usr/local/lib/libflatbuffers.a
COPY --from=neomantra/flatbuffers /usr/local/lib/cmake/flatbuffers /usr/local/lib/cmake/flatbuffers
```


Usage As A Build Tool
=====================

You can generate files on the host through volume bind mounts.  The idea is that you mount a source directory into the container, invoke `flatc` on a schema in it, and write the output to a mounted destination.

The `ENTRYPOINT` for this image is `/usr/local/bin/flatc` so you just need to follow with arguments

Example:

```
# Input on host is in /my/src.
# Output written to /my/dest.
docker run -v /my/src:/src -v /my/dest:/dest neomantra/flatbuffers --cpp --scoped-enums -o /dest /src/monster.fbs 
```


Files In This Image
===================

This image is slim, containing the `debian-slim` [base image](https://hub.docker.com/_/debian/) and the build artifacts of FlatBuffers:

```
/usr/local/bin/flatc

/usr/local/include/flatbuffers
/usr/local/include/flatbuffers/idl.h
/usr/local/include/flatbuffers/registry.h
/usr/local/include/flatbuffers/reflection.h
/usr/local/include/flatbuffers/flexbuffers.h
/usr/local/include/flatbuffers/flatc.h
/usr/local/include/flatbuffers/minireflect.h
/usr/local/include/flatbuffers/base.h
/usr/local/include/flatbuffers/grpc.h
/usr/local/include/flatbuffers/flatbuffers.h
/usr/local/include/flatbuffers/reflection_generated.h
/usr/local/include/flatbuffers/hash.h
/usr/local/include/flatbuffers/stl_emulation.h
/usr/local/include/flatbuffers/util.h
/usr/local/include/flatbuffers/code_generators.h

/usr/local/include/flatbuffers/idl_parser.cpp
/usr/local/include/flatbuffers/idl_gen_text.cpp

/usr/local/lib/libflatbuffers.a

/usr/local/lib/cmake/flatbuffers/FlatbuffersConfig.cmake
/usr/local/lib/cmake/flatbuffers/FlatbuffersConfigVersion.cmake
/usr/local/lib/cmake/flatbuffers/FlatbuffersTargets.cmake
/usr/local/lib/cmake/flatbuffers/FlatbuffersTargets-release.cmake
/usr/local/lib/cmake/flatbuffers/FlatcTargets.cmake
/usr/local/lib/cmake/flatbuffers/FlatcTargets-release.cmake
```


Customizing The Docker Image Build
===================================

The Docker image build can be customized using the following build arguments.  They are activated by applying `--build-arg <arg>=<value>` to your `docker build` run.


| Arg | Default | Description |
:----- | :-----: |:----------- |
|FLATBUFFERS_ARCHIVE_BASE_URL | https://github.com/google/flatbuffers/archive | URL to download the flatbuffers archive from |
|FLATBUFFERS_ARCHIVE_TAG | master | Tag to download |
|FLATBUFFERS_BUILD_TYPE | Release | CMake build type (e.g. Release, Debug) |
|FLATBUFFERS_USE_CLANG | false | Set to exactly `"true"` to build with `clang` instead of `gcc` |


Authors
=======

This tooling was created by Evan Wies and sponsored by [Neomantra](https://www.neomantra.com).


Copyright & License
===================

This software is released under the [MIT License](https://en.wikipedia.org/wiki/MIT_License).

Copyright (c) 2018, Neomantra BV and Evan Wies <evan@neomantra.net>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

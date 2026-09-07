# ffmpeg-ffv1
This filter decodes FFV1, the lossless intra-frame video codec, using a reduced version of ffmpeg carrying that decoder only.

## Requirements

[CMake](https://cmake.org/) is used as a build system. To install it, follow
[Debian build instructions](developing_in_debian.md).

[Emscripten SDK](https://emscripten.org/) is required for building
WebAssembly artifacts. To install it, follow the
[Download and Install](https://emscripten.org/docs/getting_started/downloads.html)
guide:

```bash
cd $OPT

# Get the emsdk repo.
git clone https://github.com/emscripten-core/emsdk.git

# Enter that directory.
cd emsdk

# Download and install the latest SDK tools.
./emsdk install latest

# Make the "latest" SDK "active" for the current user. (writes ~/.emscripten file)
./emsdk activate latest
```

## Building the accessor

```bash
# Setup EMSDK and other environment variables. In practice EMSDK is set to be
# $OPT/emsdk.
source $OPT/emsdk/emsdk_env.sh

# Assuming you are in the root level of the cloned repo :
emcmake cmake .
emmake make
```

Once built, you can use and distribute ffmpeg-ffv1_1.wasm with your universal tags.

## Rebuilding the ffmpeg libraries

```bash
emconfigure $FFMPEG_SRC/configure --target-os=none --arch=x86_32 \
    --enable-cross-compile --disable-x86asm --disable-inline-asm \
    --disable-stripping --disable-programs --disable-doc \
    --disable-runtime-cpudetect --disable-autodetect --disable-pthreads \
    --pkg-config-flags="--static" --nm="$EMSDK/upstream/bin/llvm-nm" \
    --ar=emar --ranlib=emranlib --cc=emcc --cxx=em++ --objcc=emcc --dep-cc=emcc \
    --enable-pic --disable-everything --enable-decoder=ffv1
emmake make
```

`--disable-everything` is what keeps the accessor at 774 Ko; the same source
built with ffmpeg's defaults produces a 14,8 Mo one.

## Note

FFV1 is the one video codec whose reference implementation *is* ffmpeg: it was
designed inside it and there is no second decoder to build against. GPAC has
had `GF_CODECID_FFV1` all along, but `ff_common.c` upstream has no entry for
it, so nothing could route an FFV1 pid to `ffdec`; the mapping is added here,
and `avidmx` names the `FFV1` compressor fourcc.

## Documentation

For more details, please visit our documentation at https://bevara.com/documentation/develop/.

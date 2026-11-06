# FFmpeg corresponding source (LGPL)

Complete source for a patched build of FFmpeg, published as the corresponding source required by
the GNU Lesser General Public License. Each entry pins the exact upstream revision, the patch
applied on top of it, and the license-relevant build configuration, so the source of any
prebuilt binary can be reproduced and verified.

## FFmpeg 5.1.6

|  |  |
|---|---|
| Upstream | <https://ffmpeg.org> — version `5.1.6`, git tag `n5.1.6`, commit `6338d19eb349e23a2004eb6cdd863bdef454572d` |
| Tarball | `ffmpeg-5.1.6.tar.xz`, sha256 `f4fa066278f7a47feab316fef905f4db0d5e9b589451949740f83972b30901bd` |
| Modified | Yes — two patches, applied in this order (they touch disjoint files): |
| | 1. [`patches/5.1/hevc-flv.patch`](patches/5.1/hevc-flv.patch) — HEVC (codecid 12) and Opus (codecid 13) over FLV/RTMP; `libavformat/flv.h`, `flvdec.c`, `flvenc.c`. Adapted for the 5.1 API from the community patch by [kn007](https://github.com/kn007/patch) |
| | 2. [`patches/5.1/av1-mpegts.patch`](patches/5.1/av1-mpegts.patch) — demuxing of AV1 in MPEG-TS (the stream was dropped as `bin_data`); `libavformat/demux.c`, `av1dec.c`, `mpegts.c`, `mpegtsenc.c` |
| License | GNU LGPL v2.1 or later; built with `--enable-version3`. Texts: [`COPYING.LGPLv2.1`](COPYING.LGPLv2.1), [`COPYING.LGPLv3`](COPYING.LGPLv3) |

The upstream tarball, its upstream GPG signature, and both patches are attached to the
[`ffmpeg-5.1.6` release](../../releases/tag/ffmpeg-5.1.6), so the complete source is obtainable
from here without relying on any other host.

### Reconstruct

```sh
echo "f4fa066278f7a47feab316fef905f4db0d5e9b589451949740f83972b30901bd  ffmpeg-5.1.6.tar.xz" | shasum -a 256 -c
tar xf ffmpeg-5.1.6.tar.xz
cd ffmpeg-5.1.6
patch -p1 < ../hevc-flv.patch
patch -p1 < ../av1-mpegts.patch
```

Equivalently, `git clone --depth=1 --branch n5.1.6 https://github.com/FFmpeg/FFmpeg.git` then
`git apply` the patches. The files the patches touch are identical in the tarball and the tag.

### Build configuration

```
--disable-gpl  --disable-nonfree  --enable-version3
--enable-shared  --disable-static
--enable-zlib  --enable-libdav1d  --enable-libopus  --enable-libvpx  --enable-libxml2
```

No GPL-only external library and no non-free component is enabled. Feature sets (encoders,
avfilter, programs) differ between artifacts; everything else is standard upstream `configure`.

The external libraries are permissively licensed and linked statically into the FFmpeg shared
libraries, so each artifact ships only the `libav*`/`libsw*` libraries (and, for the command-line
packages, `ffmpeg`/`ffprobe`/`ffplay` and SDL2 where applicable). They are unmodified upstream
releases:

| Library | Version | License | Source |
|---|---|---|---|
| dav1d | 1.5.1 | BSD-2-Clause | <https://code.videolan.org/videolan/dav1d>, tag `1.5.1` |
| Opus | 1.5.2 | BSD-3-Clause | <https://downloads.xiph.org/releases/opus/opus-1.5.2.tar.gz> |
| libvpx | 1.15.0 | BSD-3-Clause | <https://chromium.googlesource.com/webm/libvpx>, tag `v1.15.0` |
| libxml2 | 2.15.4 | MIT | <https://download.gnome.org/sources/libxml2/2.15/libxml2-2.15.4.tar.xz>, sha256 `98087fd181d9070724f3fbc65c7377db03038eb92bd882374daff44940138821` |
| zlib | 1.3.1 | zlib | <https://github.com/madler/zlib/releases/tag/v1.3.1> (Linux and Windows; macOS uses the system zlib) |

libxml2 provides the DASH (`.mpd`) demuxer. It is configured with
`--without-python --without-http --without-zlib --without-lzma --without-icu --without-readline --without-history --without-debug`,
and additionally `--without-iconv` on Windows.

## Scope

Only versions for which prebuilt binaries are distributed are listed here. New versions and
patch changes are added before the corresponding build ships.

## Trademark

FFmpeg is a trademark of Fabrice Bellard. This is an unofficial modified copy published for LGPL
compliance, neither endorsed by nor affiliated with the FFmpeg project. Report FFmpeg issues
upstream.

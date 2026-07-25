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
| Modified | Yes — `libavformat/flv.h`, `libavformat/flvdec.c`, `libavformat/flvenc.c`; see [`patches/5.1/hevc-flv.patch`](patches/5.1/hevc-flv.patch) |
| Patch origin | Adapted for the 5.1 API from the community patch by [kn007](https://github.com/kn007/patch) |
| License | GNU LGPL v2.1 or later; built with `--enable-version3`. Texts: [`COPYING.LGPLv2.1`](COPYING.LGPLv2.1), [`COPYING.LGPLv3`](COPYING.LGPLv3) |

The upstream tarball, its upstream GPG signature, and the patch are attached to the
[`ffmpeg-5.1.6` release](../../releases/tag/ffmpeg-5.1.6), so the complete source is obtainable
from here without relying on any other host.

### Reconstruct

```sh
echo "f4fa066278f7a47feab316fef905f4db0d5e9b589451949740f83972b30901bd  ffmpeg-5.1.6.tar.xz" | shasum -a 256 -c
tar xf ffmpeg-5.1.6.tar.xz
cd ffmpeg-5.1.6
patch -p1 < ../hevc-flv.patch
```

Equivalently, `git clone --depth=1 --branch n5.1.6 https://github.com/FFmpeg/FFmpeg.git` then
`git apply` the patch. The files the patch touches are identical in the tarball and the tag.

### Build configuration

```
--disable-gpl  --disable-nonfree  --enable-version3
```

No GPL-only external library and no non-free component is enabled. Feature sets differ between
artifacts; everything else is standard upstream `configure`.

## Scope

Only versions for which prebuilt binaries are distributed are listed here. New versions and
patch changes are added before the corresponding build ships.

## Trademark

FFmpeg is a trademark of Fabrice Bellard. This is an unofficial modified copy published for LGPL
compliance, neither endorsed by nor affiliated with the FFmpeg project. Report FFmpeg issues
upstream.

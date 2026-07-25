# FFmpeg corresponding source (LGPL)

This repository provides the **complete corresponding source code** for the modified FFmpeg
libraries distributed with — or downloaded at runtime by — our applications, as required by
the GNU Lesser General Public License.

本仓库提供随我们的应用分发（或由应用在运行时下载）的**已修改 FFmpeg 库**的完整对应源码，
以履行 GNU 宽通用公共许可证（LGPL）的源码提供义务。

> The FFmpeg source is **modified**. Do not assume the shipped binaries match a pristine
> upstream release — see [Modifications](#modifications).
>
> 我们分发的 FFmpeg **经过修改**，与上游原版发布不同，见下文。

---

## FFmpeg 5.1.6

| | |
|---|---|
| Upstream project | <https://ffmpeg.org> · <https://github.com/FFmpeg/FFmpeg> |
| Upstream version | `5.1.6` (git tag `n5.1.6`, commit `6338d19eb349e23a2004eb6cdd863bdef454572d`) |
| Upstream tarball | `ffmpeg-5.1.6.tar.xz`, sha256 `f4fa066278f7a47feab316fef905f4db0d5e9b589451949740f83972b30901bd` |
| Our modification | [`patches/5.1/hevc-flv.patch`](patches/5.1/hevc-flv.patch) |
| License | GNU LGPL v2.1 or later; built with `--enable-version3`, so **LGPL v3** terms apply to the binaries. Both texts are in this repository: [`COPYING.LGPLv2.1`](COPYING.LGPLv2.1), [`COPYING.LGPLv3`](COPYING.LGPLv3) |

The pristine upstream tarball, its upstream GPG signature, and the patch are **also attached
to the [`ffmpeg-5.1.6` release](../../releases/tag/ffmpeg-5.1.6) of this repository**, so the
complete source is obtainable from here alone and does not depend on any third-party host
remaining available.

上游原版 tarball、其官方 GPG 签名与我们的补丁**同时作为本仓库 `ffmpeg-5.1.6` release 的附件**
提供，因此完整源码可仅从本处获得，不依赖任何第三方站点的存续。

### Reconstructing the exact source / 精确复现源码

From the mirrored tarball (recommended — no external dependency):

```sh
# Download ffmpeg-5.1.6.tar.xz and hevc-flv.patch from this repository's release
echo "f4fa066278f7a47feab316fef905f4db0d5e9b589451949740f83972b30901bd  ffmpeg-5.1.6.tar.xz" | shasum -a 256 -c
tar xf ffmpeg-5.1.6.tar.xz
cd ffmpeg-5.1.6
patch -p1 < ../hevc-flv.patch
```

Or from upstream git:

```sh
git clone --depth=1 --branch n5.1.6 https://github.com/FFmpeg/FFmpeg.git
cd FFmpeg
git apply /path/to/patches/5.1/hevc-flv.patch
```

Both paths yield byte-identical sources: the three files touched by the patch are identical
between the release tarball and the `n5.1.6` git tag (verified).

两条路径结果一致：补丁涉及的三个文件在 release tarball 与 `n5.1.6` git tag 中完全相同（已验证）。

### Modifications

A single patch is applied on top of upstream. It adds support for **HEVC (FLV codec id 12)**
and **Opus (FLV audio codec id 13)** inside FLV / RTMP streams, a combination used by a number
of live-streaming services but not supported by upstream FFmpeg 5.1.

在上游之上应用了一个补丁，为 FLV / RTMP 流增加 **HEVC（FLV codec id 12）** 与
**Opus（FLV audio codec id 13）** 支持——不少直播服务在用这种组合，而上游 FFmpeg 5.1 不支持。

Files changed — all in `libavformat`, which is LGPL:

```
 libavformat/flv.h    |  2 ++
 libavformat/flvdec.c | 26 +++++++++++++++++++++++---
 libavformat/flvenc.c | 28 +++++++++++++++++++++-------
 3 files changed, 46 insertions(+), 10 deletions(-)
```

The patch is adapted from the widely used community patch by
[kn007](https://github.com/kn007/patch), revised for the FFmpeg 5.1 API.

### Build configuration / 构建配置

The distributed binaries are configured as **LGPL, not GPL**:

```
--disable-gpl  --disable-nonfree  --enable-version3
```

No GPL-only external library (e.g. `libx264`, `libx265`) and no non-free component is enabled.
Feature sets differ between artifacts (a decode-only library set, a fuller library set, and a
command-line tool set), but all are built from this source with the license flags above; the
remainder are standard upstream `configure` options (`./configure --help`).

分发产物以 **LGPL 而非 GPL** 配置构建，未启用任何 GPL 专属外部库（如 libx264 / libx265）
或 non-free 组件。不同产物功能集不同（仅解码库 / 完整库 / 命令行套件三档），但都由本源码
配合上述授权标志构建，其余均为标准上游 `configure` 选项。

---

## Scope / 收录范围

This repository covers the FFmpeg versions we actually distribute. If we ship an additional
version or change a patch, the corresponding source is added here before that build is released.

本仓库只收录我们实际分发的 FFmpeg 版本。若新增版本或改动补丁，对应源码会在该构建发布前加入。

## Trademark and affiliation / 商标与关联声明

FFmpeg is a trademark of Fabrice Bellard. This repository is an unofficial, modified copy
published solely to comply with the LGPL; it is neither endorsed by nor affiliated with the
FFmpeg project. Please report problems with the *patch* here, and problems with FFmpeg itself
to upstream.

FFmpeg 是 Fabrice Bellard 的商标。本仓库是为履行 LGPL 义务而发布的非官方修改副本，
与 FFmpeg 项目无关联、未获其背书。补丁相关问题请提到本仓库，FFmpeg 自身问题请提到上游。

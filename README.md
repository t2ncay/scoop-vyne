# Vyne Scoop bucket

Scoop manifests for [Vyne](https://github.com/t2ncay/vyne), a scientific numerical
language with native compilation, device computing, embedded firmware, plotting,
and desktop graphics.

Bucket repository: **https://github.com/t2ncay/scoop-vyne**

## Install

Once the release package and completed manifest have been published:

```powershell
scoop bucket add vyne https://github.com/t2ncay/scoop-vyne
scoop install vyne/vyne
vynec --version
```

GCC is installed as a dependency because Vyne's default execution path compiles
and runs programs. CMake is suggested for raylib graphics; Arduino CLI is
suggested for ESP32/AVR firmware. Windows graphics also require a toolchain
providing `mingw32-make`.

## Manifest status

`bucket/vyne.json` is a **draft**, adapted from `bucket/app-name.json.template`.
Its version, download URL and SHA256 are intentionally empty. At preparation
time, Vyne's latest release (`v0.7.0`) has no downloadable assets. These fields
must describe a real, tested Windows release before installation can work.

The proposed release filename convention is:

```text
vyne-VERSION-windows-x64.zip
```

The `autoupdate` URL assumes that convention and `vVERSION` GitHub release tags.
Adjust it if the actual release naming differs. Build the executable and its
runtime/modules from the same tag. A release of current main features should use
the appropriate new version rather than relabel an older tagged build.
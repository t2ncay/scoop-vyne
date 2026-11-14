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

### Complete the Vyne package

Publish the Windows ZIP in **t2ncay/vyne Releases**, not in this bucket. Preserve
the installation layout required by `vynec.exe`, including runtime headers,
external modules, `share/vyne` templates/hardware data, required vendored files,
licenses, and runtime DLLs. An executable-only archive is insufficient for
compilation, imports, initialization and native graphics builds.

Fill in these fields in `bucket/vyne.json`:

- `version`: the packaged release version, without the tag's leading `v`.
- `architecture.64bit.url`: the published release asset URL.
- `architecture.64bit.hash`: the ZIP's SHA256.

```powershell
(Get-FileHash -Algorithm SHA256 .\vyne-VERSION-windows-x64.zip).Hash.ToLower()
```

The manifest's `bin` creates a `vynec` shim. It does not add the entire package
directory to PATH. Only x64 is declared; add other architectures after publishing
and testing matching packages.

## Add another manifest

```powershell
Copy-Item bucket/app-name.json.template bucket/app-name.json
```

Replace the template fields with the application's actual metadata, supported
architectures, URLs, hashes, dependencies and executable names. Remove unused
fields. See Scoop's [App Manifests guide](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests).

## Test before publishing

From a completed bucket checkout with Scoop installed:

```powershell
Get-Content bucket/vyne.json -Raw | ConvertFrom-Json
scoop install .\bucket\vyne.json
vynec --version
vynec --help
```

Test a compiled program, external imports, project initialization, PNG plotting,
and the first native graphics build on a clean Windows machine. Verify update
and uninstall behavior too. Keep the helper scripts and workflows supplied by
[Scoop's BucketTemplate](https://github.com/ScoopInstaller/BucketTemplate).

`bin/auto-pr.ps1` targets **`t2ncay/scoop-vyne:master`**, matching this bucket's
default branch. Its upstream Scoop helper is invoked only when you run the script.

## Publish and index

Copy these prepared files to the root of your **scoop-vyne** checkout. Complete
and test the release manifest before committing/pushing it there.

To request indexing, open the repository's About settings on GitHub and add the
topic **`scoop-bucket`**. This preparation does not change GitHub settings.

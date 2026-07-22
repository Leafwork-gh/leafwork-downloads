# Leafwork PBR — Downloads

Public hosting for **Leafwork PBR** release binaries. The downloads offered at
**[leafwork.io/pbr/download](https://leafwork.io/pbr/download)** are served from
this repository's [Releases](../../releases).

Leafwork PBR turns a few photographs of a real leaf, lit from different
directions, into a PBR texture set — normal, albedo, height, and opacity maps —
for Blender, Substance, and game engines.

## What this repository is (and isn't)

- It hosts **release binaries only** (Flatpak + AppImage for Linux today;
  Windows installers later). It contains **no source code**.
- **Leafwork PBR is proprietary and closed-source.** The application binaries,
  and the contents of this repository, are **© Leafwork — all rights reserved**.
  Use of the software is governed by the
  [EULA](https://leafwork.io/legal/eula/). Nothing here is offered under an
  open-source license.

## Download

Two Linux formats, each always at a stable URL that never changes between
releases:

**Flatpak** (recommended — sandboxed, with desktop integration):

```
https://github.com/Leafwork-gh/leafwork-downloads/releases/latest/download/Leafwork_PBR-x86_64.flatpak
```

```bash
flatpak install --user Leafwork_PBR-x86_64.flatpak
```

**AppImage** (fallback single-file build, for distros where Flatpak isn't an option):

```
https://github.com/Leafwork-gh/leafwork-downloads/releases/latest/download/Leafwork_PBR-x86_64.AppImage
```

```bash
chmod +x Leafwork_PBR-x86_64.AppImage
./Leafwork_PBR-x86_64.AppImage
```

Or browse every version on the [Releases page](../../releases). Model weights
download on first run; nothing else to install.

## Maintainers — cutting a release

1. Build the artifacts from the (private) app repo:
   - **Flatpak** — built in CI by `package.yml` (KDE runtime). Dispatch it, then
     download the `leafwork-pbr-linux-flatpak` artifact and rename it to the
     asset name:
     ```bash
     gh workflow run package.yml --ref main --repo Leafwork-gh/leafwork-pbr
     # once the run is green:
     gh run download <run-id> --repo Leafwork-gh/leafwork-pbr --name leafwork-pbr-linux-flatpak
     mv leafwork-pbr.flatpak Leafwork_PBR-x86_64.flatpak
     ```
   - **AppImage** — built by the local kit:
     ```bash
     packaging/linux_appimage/build.sh          # -> dist/Leafwork_PBR-x86_64.AppImage
     ```
2. Publish both here. **The tag must match `version` in the website's
   `public/pbr/latest.json`.** Use a full release (not a pre-release) so the
   `releases/latest/download/…` URLs resolve:
   ```bash
   gh release create v0.0.1 \
     --repo Leafwork-gh/leafwork-downloads \
     --title "Leafwork PBR 0.0.1" \
     --notes "First Linux preview build." \
     Leafwork_PBR-x86_64.flatpak dist/Leafwork_PBR-x86_64.AppImage
   ```
   (To add a format to an existing tag, use `gh release upload <tag> <file>`.)
3. In `leafwork-website`, bump `version` in
   [`public/pbr/latest.json`](https://github.com/Leafwork-gh/leafwork-website/blob/main/public/pbr/latest.json)
   to match and confirm `downloads.flatpak` / `downloads.appimage` point at the
   URLs above; commit. That single change lights up both the download page and
   the in-app update banner (see the app's `docs/specs/update_manifest.md`).

Because the download link uses `releases/latest/download/…`, the URL never
changes between releases — only `latest.json`'s `version` does.

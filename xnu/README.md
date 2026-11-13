# XNU for sweet

Built on GitHub's macOS arm64 runners by `.github/workflows/xnu.yml`, using
[blacktop/darwin-xnu-build](https://github.com/blacktop/darwin-xnu-build) (MIT) at a pinned commit.

- `MACOS_VERSION=13.5` -> xnu-8796 (same generation as iOS 16.6/16.7, the last iOS for iPhone 8).
- `patches/*.patch` are copied into the build script's patch dir and applied to the xnu tree with `git apply`.
- Output: `kernel.<config>.vmapple` (later: our own SWEET machine config) as a workflow artifact.

Fetch the latest build: `gh run download -R dipa-studio/ios-sweet -n xnu-kernel -D xnu/out`

## A54X / iOS 26.2 kernel track

The Samsung Galaxy A54 5G work uses a separate experimental build track:

- Source: XNU `xnu-12377.61.12` (Darwin 25.2 / iOS 26.2 generation).
- Target: `DEVELOPMENT ARM64 A54X`, with the initial ARM64 machine config in
  `patches-a54x/`.
- Build: manually run `.github/workflows/xnu-a54x.yml` after pushing the branch
  containing it. The artifact is a kernel only; it is not a Samsung boot image
  or a bootable iOS 26.2 system.

Do not apply the XNU 8796 sweet patch series to this source. Keep A54X hardware
facts and board-specific code separate until verified from the actual handset.

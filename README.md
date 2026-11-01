# ios-class-dump (cloud build)

Builds [okanon/class-dump.ios](https://github.com/okanon/class-dump.ios) on a
GitHub-hosted macOS runner and publishes a Theos `.deb` for
`arm64` + `arm64e` iOS.

## Why

`class-dump` cannot be built on the device itself: `odcctools` / `clang` /
`ld64` all depend on `llvm-dev`, which has no `iphoneos-arm64e` candidate on
roothide. The macOS runner has Xcode's iPhoneOS SDK, so Theos can cross-compile
it properly.

## Use

Trigger the `build-class-dump` workflow, then grab the `class-dump-deb`
artifact:

```sh
gh run download -R doimty/ios-class-dump -n class-dump-deb -D out
```

Install on the device:

```sh
scp out/*.deb root@device:/var/mobile/
ssh root@device 'dpkg -i /var/mobile/class-dump_*.deb'
class-dump --help
```

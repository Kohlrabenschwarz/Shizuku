# Shizuku 13.6.1 — Android 15 / HyperOS compatibility fork

> **Unofficial fork.** This repository is not maintained, endorsed, or supported by RikkaApps. For the official project, documentation, and releases, visit [RikkaApps/Shizuku](https://github.com/RikkaApps/Shizuku) and [shizuku.rikka.app](https://shizuku.rikka.app/).

This fork contains a focused compatibility fix for Shizuku user services on affected Xiaomi/HyperOS devices, particularly MediaTek devices running Android 15. On those systems, `LoadedApk.makeApplication()` can throw a `NullPointerException` while an app such as MT Manager starts its Shizuku user service. The client then reports a misleading `shell request timed out` error.

The user-service launcher now falls back to the package `Context` when the ROM cannot create the target application's `Application` object. This lets compatible user services continue to start while retaining the normal path on unaffected devices.

## Included changes

- Added a package-context fallback for the Xiaomi/MediaTek `LoadedApk.makeApplicationInner()` failure.
- Correctly handle five-second Binder wait timeouts in the manager provider and permission activity.
- Updated the project for an archive checkout without Git history.
- Set the fork version to **13.6.1** (`versionCode 1087`).
- Kept Android 16 / API 36 as the compile and target SDK while supporting Android 7.0 and later.
- Added a consistent Java/Kotlin 21 target for reproducible builds.

The compatibility work follows the diagnosis in [RikkaApps/Shizuku#1198](https://github.com/RikkaApps/Shizuku/issues/1198) and the approach proposed in [RikkaApps/Shizuku-API#299](https://github.com/RikkaApps/Shizuku-API/pull/299).

## Building

Requirements:

- JDK 21 or newer
- Android SDK Platform 36 and Build Tools 36.0.0
- Android NDK 29
- CMake 3.31 or a compatible installed version

Build a debug APK:

```shell
./gradlew :manager:assembleDebug
```

Build a release APK:

1. Create a private signing key.
2. Copy `signing.properties.example` to `signing.properties` and enter the local key details.
3. Run `./gradlew :manager:assembleRelease`.

Signing keys, signing properties, APKs, mappings, and build outputs are intentionally excluded from version control. APKs signed with a different certificate cannot be installed over the official Shizuku package or another locally signed build.

If the exact NDK revision declared by the project is unavailable, select a compatible installed NDK explicitly:

```shell
./gradlew :manager:assembleRelease -Pshizuku.ndkVersion=29.0.13846066
```

## Credits

- [RikkaApps/Shizuku](https://github.com/RikkaApps/Shizuku) — original application and server implementation.
- [RikkaApps/Shizuku-API](https://github.com/RikkaApps/Shizuku-API) — API, provider, shared server, and `rish` components.
- [zacharee](https://github.com/zacharee) — MediaTek/Xiaomi failure investigation in upstream issue #1198.
- [thedjchi](https://github.com/thedjchi) — package-context fallback proposed in Shizuku-API pull request #299.
- The Android Open Source Project and all upstream dependency authors.

All trademarks, names, icons, and upstream copyrights belong to their respective owners. Do not present builds from this repository as official Shizuku releases.

## Licensing

The Shizuku source and the original modifications made for this fork are distributed under the **Apache License 2.0**. The license text is available in [`LICENSE`](LICENSE) and [`LICENSE-APACHE-2.0`](LICENSE-APACHE-2.0), and attribution is recorded in [`NOTICE`](NOTICE). Vendored Shizuku-API components retain their upstream license and copyright notices in the [`api`](api) directory.

---

Development and documentation for this fork were assisted by artificial intelligence.

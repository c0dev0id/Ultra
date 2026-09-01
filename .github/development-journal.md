# Development Journal

## Software Stack

- **Language**: Java 8 (source/target compatibility)
- **Build**: Gradle 4.1.3 + Android Gradle Plugin, JDK 17 in CI
- **Min SDK**: 23 (Android 6.0) / Target SDK: 30
- **CI/CD**: GitHub Actions — builds are exclusively handled there (AGP not accessible locally)

## Key Decisions

### No Jetpack / No modern Android architecture
Intentional. The goal is a 22kB APK. ViewModels, Fragments, LiveData, Hilt etc. are all off the table unless APK size is no longer a constraint.

### `LauncherApps` instead of plain `Intent` for app launch
Required for correct multi-user / work profile support. A plain `startActivity` with a MAIN/LAUNCHER intent breaks on devices with work profiles.

### Signing via env vars in `build.gradle`
Release signing config reads `SIGNING_KEYSTORE_PATH`, `SIGNING_KEYSTORE_PASSWORD`, `SIGNING_KEY_ALIAS`, `SIGNING_KEY_PASSWORD` from the environment. Secrets are stored in GitHub Actions; the keystore is base64-decoded at build time. This means `assembleRelease` always requires these env vars — debug builds are unaffected.

### Pre-release per commit
Every push to `master` creates a tagged GitHub pre-release (`prerelease-{sha}`) with the signed APK attached. Tags accumulate; no automatic cleanup is configured.

## Core Features

- 6 configurable home screen app shortcuts (persisted via `SharedPreferences`)
- Full-screen app drawer with real-time search
- Gesture controls: swipe up/down/left/right, long-press, double/triple-tap
- Multi-user / work profile aware app enumeration and launch
- `FakeHomeActivity` to trigger the Android default launcher picker

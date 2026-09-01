# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed
- Updated build toolchain to AGP 8.5 / Gradle 8.7; now targets Android 14 (API 34).
- Replaced deprecated `toggleSoftInput` with `showSoftInput` in the app drawer keyboard flow.
- Fixed `resolveActivity` call to use `MATCH_DEFAULT_ONLY` flag when detecting the current default launcher.

### Added
- GitHub Actions workflow that builds and publishes a signed pre-release APK on every push to `master`.

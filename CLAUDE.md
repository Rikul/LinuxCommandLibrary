# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Linux Command Library is a Kotlin Multiplatform (KMP) reference application for Linux commands with 6,279 manual pages. It runs fully offline across Android, iOS, Desktop (JVM), CLI (native), and Web platforms.

## Build Commands

Requires JDK 17.

```bash
# Run tests (used in CI)
./gradlew :common:testDebugUnitTest :common:desktopTest

# Build Android APK
./gradlew assembleRelease

# Build CLI (native executable named 'lcl')
./gradlew :cli:linkReleaseExecutableLinuxX64      # Linux x64
./gradlew :cli:linkReleaseExecutableLinuxArm64    # Linux arm64
./gradlew :cli:linkReleaseExecutableMacosX64      # macOS x64
./gradlew :cli:linkReleaseExecutableMacosArm64    # macOS arm64
./gradlew :cli:linkReleaseExecutableMingwX64      # Windows x64

# Build Desktop packages
./gradlew :desktopApp:packageDmg      # macOS
./gradlew :desktopApp:packageMsi      # Windows
./gradlew :desktopApp:packageDeb      # Linux DEB
./gradlew :desktopApp:packageRpm      # Linux RPM

# Code formatting (ktlint)
./gradlew spotlessCheck    # Check formatting
./gradlew spotlessApply    # Fix formatting

# Check for dependency updates
./gradlew dependencyUpdates
```

## Project Structure

```
├── common/          # Shared Kotlin code (parsing, models, utilities)
├── composeApp/      # Jetpack Compose UI (shared across Android/iOS/Desktop)
├── android/         # Android app wrapper
├── cli/             # Native CLI app using Mordant TUI library
├── desktopApp/      # Desktop JVM app wrapper
├── websiteBuilder/  # Static site generator for web version
├── iosApp/          # iOS Swift/Xcode project
└── assets/          # Markdown content (commands/, basics/, tips.md)
```

## Architecture

**Kotlin Multiplatform with expect/actual pattern:**
- Platform-specific code uses `expect` declarations in `commonMain` with `actual` implementations per platform
- Key interfaces: `AssetReader`, `PreferencesStorage`, `ShareHandler`, `ReviewHandler`

**Dependency Injection:**
- Uses Koin with `commonModule` for shared services and `platformModule()` for platform-specific implementations
- DI setup in `composeApp/src/commonMain/kotlin/di/Modules.kt`

**Data Layer:**
- Content stored as markdown files in `assets/` directory
- Repositories (`CommandsRepository`, `BasicsRepository`, `TipsRepository`) handle parsing and caching
- CLI embeds assets as generated Kotlin code at build time (see `generateEmbeddedAssets` task in cli/build.gradle.kts)

**Navigation:**
- Screen-based navigation with `Screen` sealed class in `composeApp/src/commonMain/kotlin/Navigation.kt`

## Version Management

App version is defined in `gradle/libs.versions.toml` under `appVersion`. The `generateVersionFile` task in `common/build.gradle.kts` generates `Version.kt` automatically during compilation.

## Module Dependencies

```
android/desktopApp/iosApp → composeApp → common
cli → common
websiteBuilder (standalone)
```

# Blockbench Mobile (Unofficial)

An **unofficial Android port** of [Blockbench](https://www.blockbench.net/), packaged as a native Android app so it can be installed and used directly on tablets/phones — no desktop required.

This project is **not affiliated with, endorsed by, or supported by** the original Blockbench project or its author.

## Credits

All modeling/animation logic, UI, and core functionality belong to the original project:

- **Original project:** [Blockbench](https://github.com/JannisX11/blockbench) by [JannisX11](https://github.com/JannisX11) and contributors
- **This fork:** built on top of the official `next` (development) branch

Huge thanks to the Blockbench team for building and maintaining such a well-structured, actively developed tool — and for shipping a browser-based (web) build target that made this port possible in the first place.

## What this project actually does

Blockbench officially ships as:
- A desktop app (Electron), and
- An official web beta (browser-only, hosted separately)

This project does **not** modify Blockbench's core logic. Instead, the GitHub Actions workflow in this repo:

1. Checks out the official `next` branch source
2. Runs Blockbench's own `npm run build-web` command (the same command used to produce the official web build)
3. Copies the resulting static web build (HTML/CSS/JS assets — no Electron code included) into a minimal Android app
4. The Android app displays this web build in a `WebView`, served locally via `androidx.webkit`'s `WebViewAssetLoader` (needed because the build uses ES Modules, which cannot be loaded directly from `file://` URLs)

In short: **no Blockbench source code is altered.** This repo only adds a thin Android wrapper around the official, unmodified web build output.

## Why an unofficial fork?

The official desktop build (`.exe`) isn't usable on Android tablets, and there was no ready-made native app wrapping the official web build. This project exists to fill that gap for personal/tablet use.

## Download & Installation

APKs are distributed **only via [GitHub Releases](../../releases)** on this repository. This app is **not** published on the Google Play Store or F-Droid.

> ⚠️ **Install at your own risk.** This is an unofficial, community-built APK. Always verify what you're installing — see the verification section below.

### Understanding release naming

This repository has **two different kinds of GitHub Releases**, and it's important not to confuse them:

- **`build-N`** (e.g. `build-42`) — these are created **automatically** by the CI workflow on every push to `next`. They're raw, untested debug builds meant for quick verification/testing only, and are **not** considered stable or officially recommended.
- **`vN.N.N`** (e.g. `v1.2.0`) — these are **official, manually-tagged releases**. A `vN.N.N` release is **not** produced by the automated build pipeline — it represents a version that has been deliberately reviewed and published as a proper release.

**In short: if a release is tagged `vN.N.N`, it was not just auto-uploaded by GitHub Actions — treat it as the actual recommended release.** The `build-N` releases exist purely as a byproduct of the CI pipeline for testing convenience.

### Verifying the APK before installing

Every release build produced by this repo's GitHub Actions workflow is automatically checked in two ways, and the results are posted in that workflow run's summary:

- **[VirusTotal](https://www.virustotal.com/)** — the built APK is uploaded and scanned against dozens of antivirus engines. A link to the scan report is included in the build summary so you can review it yourself before installing.
- **[Appetize.io](https://appetize.io/)** — the built APK is uploaded to Appetize's cloud Android emulator, producing a link you can open right in your browser (including on the same tablet you're planning to install it on) to try the app out **before** installing anything locally.

We strongly encourage checking both links for any release you install.

## Building it yourself

This project is built entirely via **GitHub Actions** — no local Android Studio or build environment is required. Pushing to (or manually triggering the workflow on) the `next` branch will:

1. Build the Blockbench web bundle from source
2. Package it into the Android project under `android/`
3. Produce a debug APK as a workflow artifact
4. Scan it with VirusTotal and upload it to Appetize.io for a browser-testable preview

See `.github/workflows/build-apk.yml` for the full pipeline.

## License

Blockbench is licensed under the **GNU General Public License v3.0 or later (GPL-3.0-or-later)**. As a derivative work, this project is distributed under the same license — see [LICENSE](./LICENSE) for the full text.

### Changes made relative to upstream Blockbench

- Added an `android/` directory containing a minimal native Android wrapper app
- Added a `.github/workflows/build-apk.yml` CI pipeline that builds the official web target and packages it as an Android APK, then runs VirusTotal/Appetize.io checks
- No changes were made to Blockbench's own source files (`js/`, `css/`, etc.)

## Disclaimer

This software is provided "as is", without warranty of any kind, express or implied. Use at your own risk. This is a hobby/unofficial project built with the help of AI assistance (Claude), running entirely from a tablet via GitHub Actions.

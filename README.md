# acgm_build

Build-only repo for **ACGM** — a fully local desktop app for managing anime / comic / game / media collections. No server, no network, all data on your machine.

The source code lives upstream at **[ObjectWang/ACGM](https://github.com/ObjectWang/ACGM)**. This repo holds nothing but the CI workflow.

[中文说明 →](README_CN.md)

## Get a build

1. Open the [build workflow](https://github.com/midormeepo/ACGM_build/actions/workflows/build.yml)
2. **Run workflow** → type a version like `v0.1.1` → **Run workflow**
3. Wait ~4–10 minutes, then check [Releases](https://github.com/midormeepo/ACGM_build/releases)

You get a **draft** release named `ACGM v0.1.1` with the installers attached. Drafts stay invisible to everyone else until you publish them.

Each run clones upstream fresh, so you always build the latest source. The version you type only names the tag and the release — it does not change the version inside the app.

## Files in a release

| File | What it is |
| --- | --- |
| `ACGM_*_x64-setup.exe` | NSIS installer — **recommended** |
| `ACGM_*_x64_en-US.msi` | MSI installer — ⚠️ currently broken, see below |
| `acgm.exe` | Portable — no install, just run it |

> ⚠️ **The MSI doesn't work right now.** ACGM keeps `acgm.db` and `images/` next to the executable, but the MSI installs into `C:\Program Files`, which a normal user cannot write to. The app panics on first launch. Use the NSIS installer or the portable exe.

## Portable

`acgm.exe` needs no installation. On first launch it creates `acgm.db` and `images/` right next to itself, so you can drop it in any folder and the data travels with it — a USB stick works. Needs WebView2, which ships with Windows 10/11.

## Requirements

- Windows 10/11
- [WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/) (preinstalled on Win10/11)

## Building locally

```bash
git clone https://github.com/ObjectWang/ACGM
cd ACGM
npm install

npm run tauri:build      # installers → src-tauri/target/release/bundle/
npm run tauri:portable   # portable exe → src-tauri/target/release/acgm.exe
```

Needs Node.js 20+, Rust stable, and on Windows the MSVC build tools plus the Windows 10/11 SDK. The first build compiles all of Tauri from scratch and takes a while.

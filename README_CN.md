# acgm_build

**ACGM** 的构建仓库 —— 一个纯本地运行的桌面应用,用于统一管理动画、漫画、游戏及其它媒体资源。不联网、不部署服务器,数据全在本机。

源码在上游仓库 **[ObjectWang/ACGM](https://github.com/ObjectWang/ACGM)**,本仓库只放 CI,不放源码。

[English →](README.md)

## 怎么拿到安装包

1. 打开 [build 工作流](https://github.com/midormeepo/ACGM_build/actions/workflows/build.yml)
2. 点 **Run workflow** → 填版本号(比如 `v0.1.1`)→ 再点 **Run workflow**
3. 等大约 4~10 分钟,然后去 [Releases](https://github.com/midormeepo/ACGM_build/releases)

会生成一个名为 `ACGM v0.1.1` 的**草稿** Release,安装包挂在里面。草稿只有你自己看得见,确认没问题再点发布。

每次构建都会重新拉取上游最新代码。**你填的版本号只用来命名 tag 和 Release,不会改应用内部的版本号。**

## Release 里有三个文件

| 文件 | 说明 |
| --- | --- |
| `ACGM_*_x64-setup.exe` | NSIS 安装包 —— **推荐用这个** |
| `ACGM_*_x64_en-US.msi` | MSI 安装包 —— ⚠️ 目前是坏的,见下 |
| `acgm.exe` | 便携版 —— 免安装,双击就跑 |

> ⚠️ **MSI 现在装完起不来。** ACGM 把 `acgm.db` 和 `images/` 存在 exe 同目录,而 MSI 会装进 `C:\Program Files`,普通用户对该目录没有写权限,启动时建不了数据库会直接崩溃。用 NSIS 安装包或便携版。

## 便携版

`acgm.exe` 不需要安装。首次启动会在自己旁边创建 `acgm.db` 和 `images/`,所以放哪个文件夹数据就跟到哪,丢 U 盘里也能用。依赖 WebView2,Win10/11 自带。

## 环境要求

- Windows 10/11
- [WebView2 运行时](https://developer.microsoft.com/microsoft-edge/webview2/)(Win10/11 已预装)

## 本地构建

```bash
git clone https://github.com/ObjectWang/ACGM
cd ACGM
npm install

npm run tauri:build      # 安装包 → src-tauri/target/release/bundle/
npm run tauri:portable   # 便携版 → src-tauri/target/release/acgm.exe
```

需要 Node.js 20+、Rust stable,Windows 上还需要 MSVC 构建工具和 Windows 10/11 SDK。首次构建要从头编译整个 Tauri,比较慢。

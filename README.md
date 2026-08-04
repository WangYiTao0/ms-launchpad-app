# MSToolbox 自更新发布仓

这里只放 **MSToolbox 本体**的构建产物(由源码仓的 CI 在打版本 tag 时自动发布),不放源码。

## 想安装 / 使用

到 [Releases](https://github.com/WangYiTao0/mstoolbox-app/releases) 下载最新版:

- **`MSToolbox-win-Setup.exe`** —— 安装版,双击安装,带开始菜单快捷方式;
- **`MSToolbox-win-Portable.zip`** —— 免安装版,解压到自己可写的目录,双击「MS 工具箱.exe」。使用说明见维护者提供的《portable 使用说明》。

两种都会自动检查并原地更新,不用手动重下。

## 其他文件是干什么的

`*.nupkg`、`releases.win.json`、`assets.win.json`、`RELEASES` 是自更新机制(Velopack)用的,程序自己会读,请勿删除或手动下载。

## 相关仓库

- 小程序发布仓(catalog 与各工具产物):[mstoolbox-dist](https://github.com/WangYiTao0/mstoolbox-dist)

# MS Launchpad 下载与自更新仓

这里只放 **MS Launchpad 本体**的构建产物（由源码仓的 CI 在打版本 tag 时自动发布），不放源码。

MS Launchpad 是公司内部小程序（工具）的总入口：按需下载各工具、打开时自动同步到最新版、
可以收藏常用工具、常驻托盘。免管理员权限。

## 想安装 / 使用

到 [Releases](https://github.com/WangYiTao0/ms-launchpad-app/releases/latest) 下载最新版
（文件名带版本号，下面用 `vX.Y.Z` 代替）：

- **`MSLaunchpad-vX.Y.Z-win-Portable.zip`** —— 免安装版（推荐）。解压到自己可写的目录
  （桌面或 D 盘下的固定文件夹，别放下载文件夹，也别放 `C:\Program Files`），
  双击根目录的 **`MS Launchpad.exe`**。
- **`MSLaunchpad-vX.Y.Z-win-Setup.exe`** —— 安装版，双击安装，带开始菜单快捷方式。

两种都会自动检查并原地更新，不用手动重下。

Portable 版根目录里还有 `Update.exe`（更新器，程序自己用，不用点）和 `current\` 文件夹（程序本体，
不用动）。更新后根目录偶尔会短暂多出一个没有空格的 `MSLaunchpad.exe`，那是更新器留下的，
程序下次打开会自己清掉——要双击的入口始终是 `MS Launchpad.exe`。

几件常被问到的事：

- **点窗口的 X 不会退出**，只是收进右下角托盘。真正退出走托盘图标右键菜单的「退出」。
- **开机自启**默认关闭，在托盘右键菜单里勾选；它记的是当前解压位置，挪了文件夹要重新勾一次。
- 下载过的工具和收藏存在 `%LocalAppData%\MSLaunchpadData`，换位置或重新解压都不会丢。
- 杀毒软件或 Windows 提示「可能存在风险」：这是内部工具没有购买代码签名证书导致的，
  请联系维护者或 IT 加白名单，别自己硬跑。只用本页 Releases 或维护者直接发的 zip。

## 其他文件是干什么的

`*.nupkg`、`releases.win.json`、`assets.win.json`、`RELEASES` 是自更新机制（Velopack）用的，
程序自己会读，请勿删除或手动下载。

## 排障：全部下载失败、日志报 CERTIFICATE_VERIFY_FAILED

症状：小程序下载报「下载失败（网络中断）」，自更新检查失败，日志里反复出现
`certificate verify failed: unable to get local issuer certificate`，但浏览器上网正常。

常见根因：这台 Windows 的证书库里缺 **Sectigo Public Server Authentication Root E46**
（2021 年启用的公开根证书，GitHub 的证书链现在走它）。Windows 的根证书靠联网按需
自动更新，更新被管死或一直没被触发过的机器（典型：公司管着的业务机）就会缺它。
已实测一例：装上该证书后下载即恢复（2026-09）。

修法（二选一，都不用管理员权限，只装到当前用户）：

1. 命令行：下载 [Sectigo-Public-Server-Authentication-Root-E46.pem](certs/Sectigo-Public-Server-Authentication-Root-E46.pem)，
   打开 cmd 进入下载目录，执行 `certutil -addstore -user Root Sectigo-Public-Server-Authentication-Root-E46.pem`；
   中途 Windows 弹「安全性警告」点「是」。
2. 图形界面：把上面的文件后缀改成 `.cer` 后双击 → 「安装证书」→ 「当前用户」→
   「将所有的证书都放入下列存储」→ 「受信任的根证书颁发机构」→ 完成。

装完重启 MS Launchpad，在下载失败处点「重试」验证。

安全说明：这是一张**公开 CA** 的根证书（微软根证书计划成员），SHA1 指纹
`EC8A396C40F02EBC4275D49FAB1C1A5B67BED29A`，可与 Sectigo 官网 / Mozilla 公开记录
核对。装它的效果只是「让这台电脑承认 Sectigo 签发的网站证书」，不授予任何人
解密流量的额外能力。若装完仍报同样的错，说明另有 SSL 拦截设备在替换证书，
需导出该机实际看到的链顶证书另行处理，请联系维护者。

## 相关仓库

- 小程序发布仓（catalog 与各工具产物）：[ms-launchpad-dist](https://github.com/WangYiTao0/ms-launchpad-dist)

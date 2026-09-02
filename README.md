# MSToolbox 自更新发布仓

这里只放 **MSToolbox 本体**的构建产物(由源码仓的 CI 在打版本 tag 时自动发布),不放源码。

## 想安装 / 使用

到 [Releases](https://github.com/WangYiTao0/mstoolbox-app/releases) 下载最新版:

- **`MSToolbox-win-Setup.exe`** —— 安装版,双击安装,带开始菜单快捷方式;
- **`MSToolbox-win-Portable.zip`** —— 免安装版,解压到自己可写的目录,双击「MS 工具箱.exe」。使用说明见维护者提供的《portable 使用说明》。

两种都会自动检查并原地更新,不用手动重下。

## 其他文件是干什么的

`*.nupkg`、`releases.win.json`、`assets.win.json`、`RELEASES` 是自更新机制(Velopack)用的,程序自己会读,请勿删除或手动下载。

## 排障:全部下载失败、日志报 CERTIFICATE_VERIFY_FAILED

症状:小程序下载报「下载失败(网络中断)」,自更新检查失败,日志里反复出现
`certificate verify failed: unable to get local issuer certificate`,但浏览器上网正常。

常见根因:这台 Windows 的证书库里缺 **Sectigo Public Server Authentication Root E46**
(2021 年启用的公开根证书,GitHub 的证书链现在走它)。Windows 的根证书靠联网按需
自动更新,更新被管死或一直没被触发过的机器(典型:公司管着的业务机)就会缺它。
已实测一例:装上该证书后下载即恢复(2026-09)。

修法(二选一,都不用管理员权限,只装到当前用户):

1. 命令行:下载 [Sectigo-Public-Server-Authentication-Root-E46.pem](certs/Sectigo-Public-Server-Authentication-Root-E46.pem),
   打开 cmd 进入下载目录,执行 `certutil -addstore -user Root Sectigo-Public-Server-Authentication-Root-E46.pem`;
   中途 Windows 弹「安全性警告」点「是」。
2. 图形界面:把上面的文件后缀改成 `.cer` 后双击 → 「安装证书」→ 「当前用户」→
   「将所有的证书都放入下列存储」→ 「受信任的根证书颁发机构」→ 完成。

装完重启本程序,在下载失败处点「重试」验证。

安全说明:这是一张**公开 CA** 的根证书(微软根证书计划成员),SHA1 指纹
`EC8A396C40F02EBC4275D49FAB1C1A5B67BED29A`,可与 Sectigo 官网 / Mozilla 公开记录
核对。装它的效果只是「让这台电脑承认 Sectigo 签发的网站证书」,不授予任何人
解密流量的额外能力。若装完仍报同样的错,说明另有 SSL 拦截设备在替换证书,
需导出该机实际看到的链顶证书另行处理,请联系维护者。

## 相关仓库

- 小程序发布仓(catalog 与各工具产物):[mstoolbox-dist](https://github.com/WangYiTao0/mstoolbox-dist)

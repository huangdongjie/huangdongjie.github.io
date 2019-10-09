title: Ubuntu 18 配置 shadowsocks 客户端开机自启
author: 黄东杰
tags:
  - linux
  - shadowsocks
categories: []
date: 2019-02-13 07:18:00
---
在 ubuntu 16 以前，可以将要开机执行的命令写在  `/etc/rc.local` 文件的 exit 0 语句之前，但是 ubuntu 18 中已经由传统的 init 模式启动改为 systemd 模式，就没有 rc.local 文件了。

<!-- more -->

systemd 包含有一整套的命令以提供非常丰富的功能，这个不做介绍，这里只介绍使用 apt 或 pip 安装shadowsocks 时如何使用 systemd 提供的 systemctl 命令配置开机启动 shadowsocks 客户端。

在 `/lib/systemd/system/` 目录下新建 shadowsocks.service 文件，`vim shadowsocks.service` 编辑文件，复制如下内容到文件，保存 & 退出：

```
[Unit]
Description=Shadowsocks Client Service
After=network.target

[Service]
Type=forking
User=root
ExecStart=/usr/local/bin/sslocal -d start -v -c /etc/ss.json

[Install]
WantedBy=multi-user.target

```

最后终端执行 `systemctl enable shadowsocks`

重启

done
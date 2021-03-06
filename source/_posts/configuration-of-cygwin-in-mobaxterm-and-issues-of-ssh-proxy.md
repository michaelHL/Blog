---
title: Mobaxterm 折腾笔记
date: 2017-10-23 18:02:41
tags:
    - Mobaxterm
    - SSH
    - Cygwin
categories: Handy Utilities
---

虽然已经不用 Mobaxterm 作为日常工具了，但为缅怀以前折腾的那段时光，特此归档。

<!-- more -->

## 札记

-  建议使用 10.0 版本的 Mobaxterm（下简称 Moba），10.2 版本的有少许问题，比如用 `apt-get` 或者 `apt-cyg` 不能用 `Ctrl-C` 停止，不能使用 [`gist-vim`][gist-vim] 插件，`git clone` 也出现各种不能名状的问题
-  配置 `Persistent home directory` 以及 `Persistent root (/) directory`，个人的路径为 `F:\WorkingDirectory\Cygwin\home`，`F:\WorkingDirectory\Cygwin`。关闭 Moba，修改 `/etc/fstab` 文件，加上行：
   {% codeblock lang:bash %}
   none /cygdrive cygdrive text,posix=0,noacl,user 0
   {% endcodeblock %}
   **并设置文件为只读**.
   关闭所谓的 POSIX 权限，否则导致由 Cygwin 生成的文件权限紊乱
-  设置环境变量，在 `~/.bashrc` 中写上
   {% codeblock lang:bash %}
   export PATH=/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/bin:/cygdrive/c/windows:/cygdrive/c/windows/system32
   {% endcodeblock %}
-  安装各种软件包，注意每次需通过 `-m` 来设置源（或者通过脚本或 `alias` 简化操作）。可用的 repo 有：
   - [http://mirrors.neusoft.edu.cn/cygwin/](http://mirrors.neusoft.edu.cn/cygwin/)
   - [http://mirrors.ustc.edu.cn/cygwin/](http://mirrors.ustc.edu.cn/cygwin/)
   - [http://mirrors.aliyun.com/cygwin/](http://mirrors.aliyun.com/cygwin/)
   - [http://mirrors.163.com/cygwin/](http://mirrors.163.com/cygwin/)

   比如这种操作：
   {% codeblock lang:bash %}
   apt-cyg -m http://mirrors.aliyun.com/cygwin/ install vim
   {% endcodeblock %}
   - 有时即便指定了 repo，但无法下载包，可能提示找不到文件，但仔细浏览 repo 里面相应的软件包版本，发现版本号不正确，手动下载 `setup.ini`（或者下载 `setup.xz` 进行解压）覆盖掉 `/home/.aptcyg/<http%3a%2f%2fmirrors.foo>` 下的 `setup.ini` 即可。
   - 有时无法直接通过 `apt-cyg install <pkg>` 来更新包，只好先 `remove` 再 `install`。
   - 建议安装的包：
     - busybox
     - coreutils
     - curl
     - **libopenssl100**
     - git
     - wget
     - vim
     - gcc-core
     - gcc-g++
     - gcc-mingw-core
     - gcc-mingw-g++
     - make

## 问题

### SSH 代理问题

让 Cygwin 中的 git 通过 [shadowsocks][shadowsocks] 代理。（假设代理端口为 1888）

主要参考：[如何为 Git 设置代理？][sg-git]

1. 建立 `socksssh`，`wrapper` 文件（假设在 `~/.ssh` 目录下建立）
   {% codeblock sockssh lang:bash %}
   #!/bin/sh
   ssh -o ProxyCommand="/home/.ssh/wrapper %h %p" "$@"
   {% endcodeblock %}
   {% codeblock wrapper lang:bash %}
   #!/bin/sh
   connect -S 127.0.0.1:1888 "$@"
   {% endcodeblock %}
1. 配置 Git：在 `~/.gitconfig` 文件中添加 / 修改内容：
   {% codeblock lang:ini %}
   [http]
       proxy = socks5://127.0.0.1:1888
   [https]
       proxy = socks5://127.0.0.1:1888
   [core]
       gitproxy = /home/.ssh/wrapper
   {% endcodeblock %}
1. 编译或拷贝 `connect`。
   - 项目地址：[https://bitbucket.org/gotoh/connect](https://bitbucket.org/gotoh/connect)
   - 下载：    [https://bitbucket.org/gotoh/connect/downloads/](https://bitbucket.org/gotoh/connect/downloads/)
   - 或直接：  [release][connect-release]
   - 将 `connect.exe` 复制到 `/usr/local/bin` 目录下（确保该路径在 `$PATH` 中）
1. 在 `~/.bashrc` 中加入
   {% codeblock lang:bash %}
   export GIT_SSH=/home/.ssh/socksssh
   {% endcodeblock %}
   重进 Moba.

上面的 `wrapper` 文件中 `-S` 表示以 `socks` 方式进行代理，也可以改成 `-H` (http)，不过可能要相应更改 `~/.gitconfig` 中的 `proxy`。

### git clone 出现 `X11 forwarding request failed on channel 0`

关系不大，倒是恼人。将文件 `/etc/ssh_config` 中 `ForwardX11 yes` 改为 `ForwardX11 no` 即可。


[gist-vim]: https://github.com/mattn/gist-vim
[shadowsocks]: https://github.com/shadowsocks
[sg-git]: https://segmentfault.com/q/1010000000118837
[connect-release]: https://github.com/michaelHL/WinUtilities/raw/master/Shun_ichi_Goto.connect.zip

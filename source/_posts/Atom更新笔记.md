---
title: Atom 更新笔记
date: 2018-03-29 15:11:04
categories: 技术
---

Unbuntu 虚拟机例行跟新的时候发现 Atom 并不能直接更新到最新版本。所以需通过下载 .deb 包来安装最新的版本。根据 stackoverflow 上对应[问题][1]中得票数最多的[答案][2]，可以使用 root 权限运行以下命令更新：
~~~~{bash}
#!/bin/bash
# Update atom from downloaded deb file
rm -f /tmp/atom.deb
curl -L https://atom.io/download/deb > /tmp/atom.deb
dpkg --install /tmp/atom.deb

# Update Atom packages if it's need to be updated
echo "***** apm upgrade - to ensure we update all apm packages *****"
apm upgrade --confirm false

exit 0
~~~~

以上命令可以被写入一个脚本，每次更新时可以直接运行脚本。这次更新，是先从官网下载最新的版本，然后直接运行 `sudo dpkg --install /download/path` 来安装的。

安装最新版本后， 在 editor 界面更新了 packages。 但是更新后 atom-beautify package 无法启动。所以不能使用。最后在使用上文命令行里 `apm upgrade --comfirm false` 命令更新 packages 后，问题得以解决。

另外，据同一个答案描述，也可以通过使用以下命令通过 `apt` 更新 Atom：
~~~~{bash}
sudo apt -y update
sudo apt -y upgrade
~~~~
这种更新方法需先添加 apt repository: `sudo add-apt-repository -y ppa:webupd8team/atom`。因为安装 Atom 时就已经添加了这个 repository， 所以理论上虚拟机可以用这种方法更新 Atom。但并没有尝试。

另：
文中[答案][2]的链接中加入了锚点。链接可以直接定位答案在页面的位置并居中显示。参考[文章][3]


04-02 Updates:
在最近一次的更新后，Atom 的 Updates 在更新 package 时经常出现错误导致无法更新。但是通过上文中的 `apm upgrade --confirm false`可以避免这些错误并且成功更新。更新时需要 root 权限（sudo）。


04-18-2019 Updates:
Atom 在 Linux 系统更新 packages 时， 经常会出现错误：
~~~~{bash}
EACCES, permission denied
~~~~
根据这篇博客[文章][4]中所说的，解决方案是运行下面的命令赋予当前用户足够的权限：
~~~~{bash}
sudo chown -R `whoami` ~/.atom
~~~~
其中 `whoami` 是当前用户名。


[1]: https://stackoverflow.com/questions/24741996/how-to-upgrade-atom-editor-on-linux
[2]: https://stackoverflow.com/questions/24741996/how-to-upgrade-atom-editor-on-linux#26759982
[3]: http://blog.sina.com.cn/s/blog_a45997290101lesd.html
[4]: https://joshhighland.com/2017/07/12/atom-ide-eacces-permission-denied-error-message/

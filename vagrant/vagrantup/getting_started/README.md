原文["GETTING STARTED"](https://www.vagrantup.com/docs/getting-started/)章节

<!-- START doctoc -->
#开始
<!-- END doctoc -->

本指导章节将带你进入第一个Vagrant工程，向你展示Vagrant为你提供的主要的基本功能。
如果你想知道Vagrant能带来哪些好处，你需要阅读["为什么是Vagrant?"]()章节。
本指导章节将使用VirtualBox作为环境，因为它是免费的，兼容所有主流平台，并且Vagrant内置了对它良好的支持。但你需要记住Vagrant可以在[其它许多虚拟工具](https://www.vagrantup.com/docs/getting-started/providers.html)上使用。
在你开始第一个Vagrant工程前，请[安装最新版本的Vagrant]()。另外，由于我们要使用[VirtualBox](https://www.virtualbox.org/)，所以你也需要安装它。

##启动和运行

>$ vagrant init hashicorp/precise64
>$ vagrant up

当你执行了上面两个命令，在你的VirtualBox中将会有一个Ubuntu 12.04 LTS 64-bit版本的虚拟机在运行。你可以使用*vagrant ssh*命令通过SSH连接到该虚拟机中。当你不再使用时，你可以使用*vagrant destroy*终止它。

原文["GETTING STARTED"](https://www.vagrantup.com/docs/getting-started/)章节

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**内容**

- [启动和运行](#%E5%90%AF%E5%8A%A8%E5%92%8C%E8%BF%90%E8%A1%8C)
- [工程设置](#%E5%B7%A5%E7%A8%8B%E8%AE%BE%E7%BD%AE)
- [Boxes](#boxes)
  - [安装box](#%E5%AE%89%E8%A3%85box)
  - [使用box](#%E4%BD%BF%E7%94%A8box)
  - [查找更多box](#%E6%9F%A5%E6%89%BE%E6%9B%B4%E5%A4%9Abox)
- [打开和连接](#%E6%89%93%E5%BC%80%E5%92%8C%E8%BF%9E%E6%8E%A5)
- [同步文件夹](#%E5%90%8C%E6%AD%A5%E6%96%87%E4%BB%B6%E5%A4%B9)
- [供应](#%E4%BE%9B%E5%BA%94)
  - [安装APACHE](#%E5%AE%89%E8%A3%85apache)
  - [PROVISION](#provision)
- [网络](#%E7%BD%91%E7%BB%9C)
  - [端口转发](#%E7%AB%AF%E5%8F%A3%E8%BD%AC%E5%8F%91)
  - [其它网络](#%E5%85%B6%E5%AE%83%E7%BD%91%E7%BB%9C)
- [共享](#%E5%85%B1%E4%BA%AB)
  - [登陆到HASHICORP'S ATLAS站点](#%E7%99%BB%E9%99%86%E5%88%B0hashicorps-atlas%E7%AB%99%E7%82%B9)
  - [共享](#%E5%85%B1%E4%BA%AB-1)
- [销毁](#%E9%94%80%E6%AF%81)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

本指导章节将带你进入第一个Vagrant工程，向你展示Vagrant为你提供的主要的基本功能。

如果你想知道Vagrant能带来哪些好处，你需要阅读["为什么是Vagrant?"](./why_vagrant)章节。

本指导章节将使用VirtualBox作为环境，因为它是免费的，兼容所有主流平台，并且Vagrant内置了对它良好的支持。但你需要记住Vagrant可以在[其它许多虚拟工具](https://www.vagrantup.com/docs/getting-started/providers.html)上使用。

在你开始第一个Vagrant工程前，请[安装最新版本的Vagrant](https://www.vagrantup.com/docs/installation/)。另外，由于我们要使用[VirtualBox](https://www.virtualbox.org/)，所以你也需要安装它。

## 启动和运行

> $ vagrant init hashicorp/precise64

> $ vagrant up

当你执行了上面两个命令，在你的VirtualBox中将会有一个Ubuntu 12.04 LTS 64-bit版本的虚拟机在运行。你可以使用`vagrant ssh`命令通过SSH连接到该虚拟机中。当你不再使用时，你可以使用`vagrant destroy`终止它。

现在想象一下以前你工作过的工程都是如此简单地启动！使用Vagrant，你可以很自在地继续之前的工作，因为你需要的任何工程、依赖、设置网络及共享文件夹等工作，都是**只需要**一个`vagrant up`命令就搞定啦！

本篇的余下部分将带你开始一个更复杂的工程，并涉及更多的Vagrant功能。

## [工程设置](https://www.vagrantup.com/docs/getting-started/project_setup.html)
设置Vagrant工程的第一步是创建一个[Vagrantfile](../vagrantfile/)。Vagrantfile有两个目的:
* 标记工程的根目录。Vagrant中的很多配置项都是相对于该根目录的。
* 描述虚拟机类型，运行工程所需要的资源，所需要的软件以及获取方法。

Vagrant提供了一个内置的命令来初始化用于Vagrant的目录:`vagrant init`。为了完成本向导，请在你的命令行终端中输入下列命令:

> mkdir vagrant_getting_started
> cd vagrant_getting_started
> vagrant init

现在你的当前目录在会生成一个Vagrantfile。你可以查看一下这个文件，里面都是一些注释及样例。如果它看起来令人生畏的话，别担心，我们马上要对它进行修改。

你也可以在一个已经存在的目录中运行`vagrant init`命令来为已存在的工程设置Vagrant环境。

如果你用版本控制工具的话，Vagrantfile应该和你的工程一起上传。这样，每个参与此工程的人都能不需要做任何前期工作(配置环境等)就能参与其中。

## [Boxes](https://www.vagrantup.com/docs/getting-started/boxes.html)

**译者注: Boxes算一个术语，所以就不翻译了**

从零开始创建一个虚拟机是个缓慢而且冗余的过程，但在Vagrant中，你可以用一个基础的镜像很快克隆出一个虚拟机。这个基础的镜像称为"boxes"，通常在你建立一个新的Vagrantfile之后的第一步就是指定你要使用的box。

### 安装box

如果你已经执行了开始时的命令(并且没有出错)，那么你之前已安装了一个box，下面的命令你就不需要再执行了。然而，你仍可以阅读这部分，这样可以了解更多管理box的细节。

`vagrant box add`用于给Vagrant添加box。这个命令将box保存在指定的全称下面以便于多个Vagrant环境可以重用它。如果你还没有添加box，你可以像下面这样做:
> vagrant box add hashicorp/precise64

这个命令会从[HashiCorp的Atlas box目录(以下称为官方目录)](https://atlas.hashicorp.com/boxes/search)中下载一个名为"hashicorp/precise64"的box。这是下载box最简单的方法，同时你也可以从本地文件、url等添加。

box存放在当前用户的全局目录中。每个工程使用一个box时都是将它作为一个初始镜像复制，并且决不会修改这个基础镜像。这意味着如果你有两个工程都使用刚刚我们添加的"hashicorp/precise64" box，那么你在任何一个虚拟机中更改文件都不会影响到另一个虚拟机。

上述命令中，你会注意到box是有命名空间的。box的名称分为两部分，用户名和box名，用斜线分隔。上面例子中，用户名是"hashicorp"，而box名是"precise64"。你也可以通过URL或者本地文件路径指定box，但超出了本向导的范围。

```
Namespaces do not guarantee canonical boxes! A common misconception is that a namespace like "ubuntu" represents the canonical space for Ubuntu boxes. This is untrue. Namespaces on Atlas behave very similarly to namespaces on GitHub, for example. Just as GitHub's support team is unable to assist with issues in someone's repository, HashiCorp's support team is unable to assist with third-party published boxes.
```

### 使用box

既然box已经被添加到Vagrant中，我们需要配置我们的工程来使用它。打开Vagrantfile并且把修改内容如下:

> Vagrant.configure("2") do |config|
>   config.vm.box = "hashicorp/precise64"
> end

本例中的"hashicorp/precise64"必须和你之前添加的box名称相同。如果之前没有添加box，Vagrant会在运行的时候自动下载并添加该box。

下一部分中，我们会打开Vagrant环境并和它进行一些交互。

### 查找更多box
在本向导的余下部分，我们会只使用之前添加的"hashicorp/precise64"这个box。但结束本向导后，你可能首先会问"我从哪里可以找到更多的box呢"。

最好的查找地点的[官方目录](https://atlas.hashicorp.com/boxes/search)。HashiCorp提供了一个公共目录，里面存放了很多可以在不同平台运行的免费的box。同时它提供了很好的搜索功能，你可以查找你关心的box。

除了查找免费box外，HashiCorp还允许你管理自己的box列表，如果你打算为自己的私有组织创建box，你也可以拥有私有的box列表。

## [打开和连接](https://www.vagrantup.com/docs/getting-started/up.html)
是时候启动你的第一个Vagrant环境啦。在终端中运行:
> vagrant up

一分钟内，这个命令就会结束并且你会拥有一个正在运行的Ubuntu。你可能不会看到什么现象，因为Vagrant运行在一个没有UI的虚拟机中。为了证明它在运行，你可以通过SSH连接到该虚拟机中:
> vagrant ssh

这个命令会带你进入一个完全成熟的SSH会话中。你可以做任何事与它交互，但你需要小心使用`rm -rf /`，因为Vagrant自动将宿主机上包含Vagrantfile的目录共享到了虚拟机的`/vagrant`目录中。下一部分中我们将涉及共享目录。

我们花点时间考虑一下刚才发生了什么: 仅仅通过一行配置和终端中一个简单的命令，我们打开了一个功能完整的，提供SSH访问的虚拟机。酷！

当你在这个机器上做了一些无用的操作后，你可以在你的宿主机上运行`vagrant destroy`来终止使用的虚拟机。

**注:** 
`vagarnt destroy`命令并不实现删除已下载的box文件。如果你要完全删除box文件，你可以用`vagrant box remove`命令。


## [同步文件夹](https://www.vagrantup.com/docs/getting-started/synced_folders.html)

虽然如果容易地拥有一个虚拟机是很酷的事，但很多用户都不愿意只是在SSH中用一些终端编辑工具编辑文件。幸运的是，用Vagrant你不需要这样。通过使用同步文件夹，Vagrant会自动地同步宿主机和虚拟机之间的共享文件。

默认情况下，Vagrant会将你的工程目录(记住，是那个包含Vagrantfile的目录)共享到虚拟机的`/vagrant`目录中。

注意当你通过`vagrant ssh`进入虚拟机时，你在`/home/vagrant`目录中。`/home/vagrant`和共享的`/vagrant`不是同一个目录。

如果你的终端显示一个关于"incompatible guest additions"(或"no guest additions")的错误时，你可能需要升级你的box或者选择其它box(如`hashicorp/precise64`)。一此用户也通过使用[`vagrant-vbguest`](https://github.com/dotless-de/vagrant-vbguest)插件成功运行，但这并不是由Vagrant团队提供的官方支持。

再次运行`vagrant up`并SSH到虚拟机中你会看到:
> $ vagrant up
> ...
> $ vagrant ssh
> ...
> vagrant@precise64:~$ ls /vagrant
> Vagrantfile

无论你是否相信，你在虚拟机中看到的这个Vagrantfile确实是你主机中的这个Vagrantfile。你可以修改它来证明。
> vagrant@precise64:~$ touch /vagrant/foo
> vagrant@precise64:~$ exit
> $ ls
> foo Vagrantfile

哇！(-_-~!)"foo"现在在你的宿主机中了。你可以看到，Vagrant保持同步目录同步。

通过[同步目录]()，你可以继续在你的宿主机在使用编辑器并将这些文件同步到虚拟机中。

## [供应](https://www.vagrantup.com/docs/getting-started/provisioning.html)

现在我们有了一个运行的ubuntu基础版本，并且我们可以在宿主机中编辑并同步到虚拟机中，让我们用一个web服务器将它们暴露出来吧。

你可以SSH进去并安装一个web服务器就可以了，但这样的话其它使用的人也不得不做同样的事。Vagrant提供了自动供应的功能。使用这功能，Vagrant会在`vagrant up`的时候自动安装软件，这样虚拟机就可以被重复创建和直接使用了。

### 安装APACHE

在我们的基础工程中，我们将只安装[Apache](http://httpd.apache.org/)，使用shell脚本安装。在Vagrantfile所在的目录创建以下shell脚本并把它保存为`bootstrap.sh`:
```shell
#!/usr/bin/env bash
apt-get update 
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```

Next, we configure Vagrant to run this shell script when setting up our machine. We do this by editing the Vagrantfile, which should now look like this:
接下来，我们配置Vagrant来在我们启动虚拟机的时候运行此脚本。更改Vagrantfile如下:
> Vagrant.configure("2") do |config|  
>   config.vm.box = "hashicorp/precise64"
>   config.vm.provision :shell, path: "bootstrap.sh"
> end

其中新增的"provision"行告诉Vagrant使用shell来设置虚拟机，用`bootstrap.sh`文件。文件路径为相对于工程根目录的相对路径。

### PROVISION

所有项都配置好后，只需要运行`vagrant up`命令来创建虚拟机，Vagrant会自动运行它。你应该可以在终端中看到shell脚本的输出。如果机器已经在运行，你可以使用`vagrant reload --provision`来迅速地重启(路过初始化的一此重要步骤)。这个"--provision"标志使Vagrant运行provisioners，而通常Vagrant只会在第一次使用`vagrant up`时做这一步骤。

Vagrant完全运行起来后，其中的web服务器也会启动并运行。你还不能从你的浏览器中看到这个网页，但你可以通过从SSH方式下载虚拟机中的一个文件来确认provision正常运行:
> $ vagrant ssh
> ...
> vagrant@precise64:~$ wget -qO- 127.0.0.1

上面这一步骤正常运行了，是因为在上面shell脚本中，我们安装了Apache并将Apache的默认`文件目录`指向了`/vagrant`目录。

你可以创建更多的文件，并在终端中查看它们。但在下一步中，我们会讲到网络配置选项，那样你就可以用浏览器对客户机进行访问。

## [网络](https://www.vagrantup.com/docs/getting-started/networking.html)

至此，我们拥有了一个运行的web服务器，并且可以从宿主机中修改文件，而且它们会被自动地同步到客机中。然而，仅仅从终端中访问这些网页文件是不足以满足需求的。在这一步骤中，我们将会使用Vagrant的网络功能来提供额外的选项，使我们可以从宿主机中访问到客机。

### 端口转发

一个选项是用于端口转发的。端口转发允许你将客机中的指定的端口映射到主机的一个端口上。这样，你可以访问你本机的一个端口，但实际上所有的网络流量都会转发到客机的指定端口上。

Let us setup a forwarded port so we can access Apache in our guest. Doing so is a simple edit to the Vagrantfile, which now looks like this:
现在我们来设置转发端口，这样我们就可以访问到客机上的Apache了。我们只需修改一下Vagrantfile即可:
> Vagrant.configure("2") do |config|
>   config.vm.box = "hashicorp/precise64"
>   config.vm.provision :shell, path: "bootstrap.sh"
>   config.vm.network :forwarded_port, guest: 80, host: 4567
> end

运行`vagrant reload`或者`vagrant up`(要看当前客机是否在运行)来使修改生效。

一旦机器再次运行，在你的浏览器中载入`http://127.0.0.1:4567`，你将会看到虚拟机中web服务器提供的网页。

### 其它网络

Vagrant还提供其它形式的网络，允许你将一个静态IP地址映射到客机，或者将客机桥接到一个已存在的网络上。如果你对其它选项感兴趣，可以阅读[这里](../networking/)。

## [共享](https://www.vagrantup.com/docs/getting-started/share.html)

现在我们已经有了一个功能十分丰富的开发环境。但为了方便地提供开发环境，Vagrant提供了便捷的共享和协作功能--[Vagrant共享](../share/)。

通过Vagrant共享功能，你可以将你的Vagrant环境通过网络分享给世界上的任何人。它会为你的Vagrant环境提供一个URL，世界上的任何一台联网的设备都可以通过网络路由到你的环境。

### 登陆到HASHICORP'S ATLAS站点

在开始分享你的Vagrant环境前，你需要在[HashiCorp's Atlas](https://www.vagrantup.com/docs/other/atlas.html)中注册一个账号。别担心，它是免费的。

注册后，你可以用`vagrant login`命令登陆:
> $ vagrant login
> Username or Email: mitchellh
> Password (will be hidden):
> You are now logged in!

### 共享

登陆后，运行`vagrant share`:
> $ vagrant share
> ...
> ==> default: Your Vagrant Share is running!
> ==> default: URL: http://frosty-weasel-0857.vagrantshare.com
> ...

不要试上面的URL，你的会不一样。从`vagrant share`的输出中拷贝出URL并在web浏览器上访问。它将会载入我们先前设置的Apache页面。

如果你修改你分享的目录中的文件并刷新URL，你会看到它更新了。这个URL会直接路由到你的Vagrant环境中，世界上任何一台连接互联网的设备都可以访问。

你可以在终端中按`Ctrl+C`来终止共享会话。

Vagrant分享比简单的HTTP分享更有效。你可以查看完整的[Vagrant分享文档](../share/)获取更多细节。

## [销毁](https://www.vagrantup.com/docs/getting-started/teardown.html)

We now have a fully functional virtual machine we can use for basic web development. But now let us say it is time to switch gears, maybe work on another project, maybe go out to lunch, or maybe just time to go home. How do we clean up our development environment?

With Vagrant, you suspend, halt, or destroy the guest machine. Each of these options have pros and cons. Choose the method that works best for you.

Suspending the virtual machine by calling vagrant suspend will save the current running state of the machine and stop it. When you are ready to begin working again, just run vagrant up, and it will be resumed from where you left off. The main benefit of this method is that it is super fast, usually taking only 5 to 10 seconds to stop and start your work. The downside is that the virtual machine still eats up your disk space, and requires even more disk space to store all the state of the virtual machine RAM on disk.

Halting the virtual machine by calling vagrant halt will gracefully shut down the guest operating system and power down the guest machine. You can use vagrant up when you are ready to boot it again. The benefit of this method is that it will cleanly shut down your machine, preserving the contents of disk, and allowing it to be cleanly started again. The downside is that it'll take some extra time to start from a cold boot, and the guest machine still consumes disk space.

Destroying the virtual machine by calling vagrant destroy will remove all traces of the guest machine from your system. It'll stop the guest machine, power it down, and remove all of the guest hard disks. Again, when you are ready to work again, just issue a vagrant up. The benefit of this is that no cruft is left on your machine. The disk space and RAM consumed by the guest machine is reclaimed and your host machine is left clean. The downside is that vagrant up to get working again will take some extra time since it has to reimport the machine and re-provision it.

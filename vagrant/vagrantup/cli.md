# 命令行接口
**[原文](https://www.vagrantup.com/docs/cli/)**

几乎所有与Vagrant的交互都是通过命令行完成的。

这些接口都通过`vagrant`命令使用，并随着Vagrant一起安装。`vagrant`命令有很多子命令，比如`vagrant up`，`vagrant destroy`等。

如果你只运行`vagrant`命令，所有可用的子命令都会在帮助菜单中显示。除此以上，你可以运行任何的Vagrant命令并加上`-h`标志，就能输出该命令的帮助信息。比如，运行`vagrant init -h`，帮助信息会显示一句功能简介和所有选项的列表。

You may also wish to consult the documentation regarding the environmental variables that can be used to configure and control Vagrant in a global way.
你还可以查询[文档](https://www.vagrantup.com/docs/other/environmental-variables.html)了解关于配置和控制Vagrant的环境变量。

## box
**[原文](https://www.vagrantup.com/docs/cli/box.html)**

### box
**命令**: `vagrant box`

这个命令用于管理(添加、删除等)box。它的功能主要由如下几个子命令完成:
- [`add`]()
- [`list`]()
- [`outdated`]()
- [`remove`]()
- [`repackage`]()
- [`update`]()

#### 添加box
**命令**: `vagrant box add ADDRESS`

这个命令根据传递给Vagrant的地址添加box。地址可以是以下三种之一:

- 一个[Vagrant可用镜像目录](https://atlas.hashicorp.com/boxes/search)的简称，如`hashicorp/precise64`
- 在一个[目录](https://atlas.hashicorp.com/boxes/search)中的文件路径或者HTTP URL。对于HTTP，支持基本的身份认证和`http_proxy`环境变量。也支持HTTPS。
- 直接指向box文件的URL。这种情况下，你必须指定一个`--name`标志(见下面)，并且版本和更新功能将不可用。

如果下载过程中出现错误或者被`Ctrl-C`中断，下次启动时Vagrant会尝试恢复下载。Vagrant只会尝试恢复初次下载后6个小时内的下载。

##### 选项
- `--box-version VALUE` - 你要添加的box版本。默认情况下会添加最新版。其中`VALUE`应该是确切的版本号，如"1.2.3"，或者一个版本集，如">= 1.0, < 2.0"。
- `--cacert CERTFILE` - 用于CA验证身份的证书。如果远程服务端用的不是一个标准的根证书，就需要设置这个选项。
- `--capath CERTDIR` - 用于CA验证身份的证书目录。同上。
- `--cert CERTFILE` - 用于下载box时的客户端证书。
- `--clean` - 如果指定了此项，Vagrant会清除之前同一个URL下载的所有临时文件。当服务器内容有更改时，你可能不希望Vagrant恢复之前的下载，这个选项就会很有用了。
- `--force` - 强制下载并覆盖同样名称的box。
- `--insecure` - 如果URL是HTTPS协议时，SSL证书将不会认证。
- `--provider PROVIDER` - 设置此项，Vagrant将会用指定的PROVIDER来添加的box，默认情况下，Vagrant自动检查合适的provider来使用。

##### 用于直接box文件的选项
下面的选项只会在你直接地(即不是用目录)添加一个box文件时有效。
- `--checksum VALUE` - 下载box文件的校验码。如果指定了，Vagrant会在下载完成后比较校验码并在不一致时抛错。由于box文件太大，强烈推荐使用这一选项。如果此项被设置，`--checksum-type`也必须被设置。如果你是从一个目录下载，校验码已经包含在目录中。
- `--checksum-type TYPE` - 指定由`--checksum`指定的校验码的类型。当前支持的类型有`md5`，`sha1`和`sha256`。
- `--name VALUE` - box的逻辑名称。这个是你将在你的Vagrantfile中设置的`config.vm.box`的值。当将一个box加入到目录时，这个名称将被放在目录条目中，不需要指定。
- 


#### 列出box
命令: `vagrant box list`
此命令列出所有已安装在Vagrant中的box。

#### 检查box是否过期
命令: `vagrant box outdated`
此命令告诉你当前Vagrant环境在用的box是否已过期。如果`--global`标志被设置，所有已安装的box都会被检查。
Checking for updates involves refreshing the metadata associated with a box. This generally requires an internet connection.

##### 选项
- `--global\    `
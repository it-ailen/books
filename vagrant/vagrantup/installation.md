# 安装Vagrant
**[原文](https://www.vagrantup.com/docs/installation/)**

安装Vagrant十分简单。进入[Vagrant下载页面](https://www.vagrantup.com/downloads.html)，选择合适的安装程序或者包即可。安装包需要使用你的平台的标准流程安装。

安装器会自动把`vagrant`加入系统路径，这样就可以在终端中直接使用。如果无法找到，请尝试先注销再重新登陆进入系统(这在windows尤其重要)。

```html
Looking for the gem install? Vagrant 1.0.x had the option to be installed as a RubyGem. This installation method is no longer supported. If you have an old version of Vagrant installed via Rubygems, please remove it prior to installing newer versions of Vagrant.
```

```html
Beware of system package managers! Some operating system distributions include a vagrant package in their upstream package repos. Please do not install Vagrant in this manner. Typically these packages are missing dependencies or include very outdated versions of Vagrant. If you install via your system's package manager, it is very likely that you will experience issues. Please use the official installers on the downloads page.
```

## 后向兼容

**[原文](https://www.vagrantup.com/docs/installation/backwards-compatibility.html)**

### FOR 1.0.X

Vagrant 1.1+ provides full backwards compatibility for valid Vagrant 1.0.x Vagrantfiles which do not use plugins. After installing Vagrant 1.1, your 1.0.x environments should continue working without modifications, and existing running machines will continue to be managed properly.

This compatibility layer will remain in Vagrant up to and including Vagrant 2.0. It may still exist after that, but Vagrant's compatibility promise is only for two versions. Seeing that major Vagrant releases take years to develop and release, it is safe to stick with your version 1.0.x Vagrantfile for the time being.

If you use any Vagrant 1.0.x plugins, you must remove references to these from your Vagrantfile prior to upgrading. Vagrant 1.1+ introduces a new plugin format that will protect against this sort of incompatibility from ever happening again.

### FOR 1.X

Backwards compatibility between 1.x is not promised, and Vagrantfile syntax stability is not promised until 2.0 final. Any backwards incompatibilities within 1.x will be clearly documented.

This is similar to how Vagrant 0.x was handled. In practice, Vagrant 0.x only introduced a handful of backwards incompatibilities during the entire development cycle, but the possibility of backwards incompatibilities is made clear so people are not surprised.

Vagrant 2.0 final will have a stable Vagrantfile format that will remain backwards compatible, just as 1.0 is considered stable.

## 升级Vagrant

**[原文](https://www.vagrantup.com/docs/installation/upgrading.html)**

如果你要升级Vagrant的1.0.x版本，请阅读[这里](https://www.vagrantup.com/docs/installation/upgrading-from-1-0.html)。本节讲述升级1.x系列版本Vagrant的一般方法。

Vagrant 1.x系列发行版的升级很直接:

1. [下载](https://www.vagrantup.com/downloads.html)新的安装包。
2. 覆盖安装已存在的包。

安装器会正确地覆盖和删除旧的文件。在升级过程中推荐不要运行其它Vagrant进程。

请注意2.0之前的Vagrantfile语法都不固定。所以尽管针对1.0.x版本的Vagrantfile照常工作(参见[此处]())，但新的Vagrantfile可能并不后向兼容。



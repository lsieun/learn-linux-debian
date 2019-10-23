# ppa

## What is ppa

URL： https://www.linuxidc.com/Linux/2012-12/75354.htm

PPA是Personal Package Archives首字母简写，也就是个人软件包集。只有Ubuntu用户可以用，而所有的PPA都是寄存在launchpad.net网站上。

很多软件包由于各种原因吧，不能进入官方的Ubuntu软件仓库。为了方便Ubuntu用户使用，launchpad.net提供了ppa,允许用户建立自己的软件仓库，自由的上传软件。PPA也被用来对一些打算进入Ubuntu官方仓库的软件，或者某些软件的新版本进行测试。

Launchpad是Ubuntu母公司canonical有限公司所架设的网站，是一个提供维护、支援或联络Ubuntu开发者的平台。

**使用PPA的好处**是Ubuntu系统中使用PPA源的软件可以让你在第一时间体验到最新版本的软件。

针对Ubuntu而言，用https://launchpad.net/ubuntu/+ppas搜索更加准确

## Adding this PPA to your system

URL:

- https://launchpad.net/~ubuntu-toolchain-r/+archive/ubuntu/test

You can update your system with unsupported packages from this untrusted PPA by adding `ppa:ubuntu-toolchain-r/test` to your system's Software Sources.

```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
```


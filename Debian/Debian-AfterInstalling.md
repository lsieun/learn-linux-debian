# Debian 9 After Installing

URL: [debian 9 安装后需做的几件事](https://www.cnblogs.com/OneFri/p/8308340.html)

## apt源更新

## 输入法fcitx(Free Chinese Input Toy of X)

Linux 下有两大输入法框架：ibus 和fcitx，其中fcitx 的体验要比ibus 好，因此选择fcitx 框架，并安装搜狗输入法。

```bash
# 删除ibus
sudo apt purge ibus

sudo apt install im-config
sudo apt install fcitx
sudo apt-get install fcitx-config-gtk
sudo apt-get install fcitx-table-wbpy
sudo apt-get install fcitx-ui-light
```

安装完后，需要重启系统，然后在终端中输入

```bash
$ fcitx-config-gtk3
```

## GVim

```bash
sudo apt install vim-gnome
```

## gnome-tweak-tool桌面管理工具

```bash
sudo apt install gnome-tweak-tool
```

## 视频播放器

```bash
sudo apt install mpv
```

## 视频录制软件

```bash
sudo apt install kazam
```

## 截图软件 shutter

```bash
sudo apt install shutter
```

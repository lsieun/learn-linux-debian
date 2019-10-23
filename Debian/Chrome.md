# Google Chrome Web Browser installation on Debian 9 Stretch Linux

URL: https://linuxconfig.org/google-chrome-web-browser-installation-on-debian-9-stretch-linux

## Download Google Chrome

First, download the latest Google Chrome's debian package using the wget command:

```bash
$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

## Install Google Chrome

Once the latest Google Chrome debian package is downloaded, use `dpkg` to perform the actual installation. Execute, the below command as **root** or with use `sudo` command:

```bash
$ sudo dpkg -i google-chrome-stable_current_amd64.deb 
Selecting previously unselected package google-chrome-stable.
(Reading database ... 300021 files and directories currently installed.)
Preparing to unpack google-chrome-stable_current_amd64.deb ...
Unpacking google-chrome-stable (55.0.2883.87-1) ...
Setting up google-chrome-stable (55.0.2883.87-1) ...
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/x-www-browser (x-www-browser) in auto mode
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/gnome-www-browser (gnome-www-browser) in auto mode
update-alternatives: using /usr/bin/google-chrome-stable to provide /usr/bin/google-chrome (google-chrome) in auto mode
Processing triggers for menu (2.1.47) ...
Processing triggers for man-db (2.7.6.1-2) ...
Processing triggers for desktop-file-utils (0.23-1) ...
Processing triggers for gnome-menus (3.13.3-8) ...
Processing triggers for mime-support (3.60) ...
```

All done. To start using Chrome Browser either click on Google Chrome shortcut by navigating to your start menu or run:

```bash
$ google-chrome
```

command from your terminal.


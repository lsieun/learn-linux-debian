# How To Install Wine 3 on Debian 9 (Stretch)

- [How To Install Wine 3 on Debian 9 (Stretch)](https://tecadmin.net/install-wine-debian-9-stretch/)

The Winehq team has announced the latest stable release 3.0 on January 18, 2018. Its source code is available for download from its official site. You may also use the **package manager** to install `wine`. Wine is an Open Source implementation of the Windows API and will always be free software. Approximately half of the source code is written by its volunteers, and remaining effort sponsored by commercial interests, especially CodeWeavers.

![WINE](https://tecadmin.net/wp-content/uploads/2013/10/winehq_logo_glass.png)

This article will help you to install **Wine 3.0 Stable Release** on Debian 9 Stretch system using the `apt-get` package manager.

## Step 1 – Prerequsiteis

First of all, If you are running with 64-bit system enable 32-bit architecture. Also, import gpg key to your system.

```bash
sudo dpkg --add-architecture i386 
wget -qO - https://dl.winehq.org/wine-builds/winehq.key | sudo apt-key add -
```

Use the following command to enable Wine apt repository in your system based on your operating system and version.

```bash
sudo apt-add-repository https://dl.winehq.org/wine-builds/debian/
```

## Step 2 – Install Wine on Debian 9

Use below commands to install Wine packages from the apt repository. The `--install-recommends` option will install all the recommended packages by `winehq-stable` on your system.

```bash
sudo apt-get update
sudo apt-get install --install-recommends winehq-stable
```

The wine packages are installed under `/opt/wine-stable` directory. So I set wine bin directory to `PATH` environment to access commands system-wide.

```bash
export PATH=$PATH:/opt/wine-stable/bin
```

## Step 3 – Check Wine Version

Wine installation successfully completed. Use the following command to check the version of wine installed on your system

```bash
$ wine --version

wine-3.0.4
```

## How to Use Wine (Optional)

To use wine we need to login to the Debian desktop system. After that download a windows `.exe` file like `PuTTY` on your system and open it with Wine as below screenshot or use following command.

```bash
wine putty.exe
```

![PuTTY](https://tecadmin.net/wp-content/uploads/2019/01/wine-debian9.png)

# Enable SSH Server on Debian

URL: [How to Enable SSH Server for Remote Login on Debian 9](https://linuxhint.com/enable-ssh-server-debian/)

<!-- TOC -->

- [Enable SSH Server on Debian](#enable-ssh-server-on-debian)
  - [1. Installing SSH Server](#1-installing-ssh-server)
  - [2. Remove SSH Server from startup](#2-remove-ssh-server-from-startup)
  - [3. SSH Cheat](#3-ssh-cheat)
    - [3.1. SSH status](#31-ssh-status)
    - [3.2. Install and Remove SSH](#32-install-and-remove-ssh)
    - [3.3. Start and Stop](#33-start-and-stop)
    - [3.4. Startup enable and disable](#34-startup-enable-and-disable)

<!-- /TOC -->

## 1. Installing SSH Server

On Debian, SSH server comes as ‘**openssh-server**’ package. To install OpenSSH on Debian, run the following command:

```bash
sudo apt install openssh-server
```

On Debian, the default behavior of OpenSSH server is that it will start automatically as soon as it is installed. It is also listening on port 22. You can also check whether OpenSSH server is running on it with the following command:

```bash
sudo systemctl status ssh
```

If in any case OpenSSH server is not running, you can run the following command to start OpenSSH server.

```bash
sudo systemctl start ssh
```

## 2. Remove SSH Server from startup

By default, on Debian, OpenSSH server should start automatically on system boot. If you don’t want it to start on boot then first stop the OpenSSH server with the following command:

```bash
sudo systemctl stop ssh
```

Now if you check the status of your OpenSSH server, you should see that it is not running.

```bash
sudo systemctl status ssh
```

Now disable OpenSSH server from startup with the following command:

```bash
sudo systemctl disable ssh
```

## 3. SSH Cheat

### 3.1. SSH status

```bash
sudo systemctl status ssh
```

### 3.2. Install and Remove SSH

To install OpenSSH on Debian:

```bash
sudo apt install openssh-server
```

### 3.3. Start and Stop

To start OpenSSH server:

```bash
sudo systemctl start ssh
```

To stop OpenSSH server:

```bash
sudo systemctl stop ssh
```

To restart the SSH server:

```bash
sudo systemctl restart ssh
```

### 3.4. Startup enable and disable

To start OpenSSH server on boot

```bash
sudo systemctl enable ssh
```

To disable OpenSSH server from startup:

```bash
sudo systemctl disable ssh
```


# How To Create a Sudo User on Debian

原文：[How To Create a Sudo User on Debian](https://linuxize.com/post/how-to-create-a-sudo-user-on-debian/)

<!-- TOC -->

- [1. Add the user to the sudo group](#1-add-the-user-to-the-sudo-group)
- [2. Test the sudo access](#2-test-the-sudo-access)
- [3. Password Timeout](#3-password-timeout)
- [4. Run a Command as a User Other than Root](#4-run-a-command-as-a-user-other-than-root)

<!-- /TOC -->

The `sudo` command (short for **Super-user do**) is a program designed to allow users to execute commands with the security privileges of another user, by default the root user.

## 1. Add the user to the sudo group

Usually, to grant sudo access to a user you need to add the user to the sudo group defined in the `sudoers` file. On **Debian**, **Ubuntu** and their derivatives, members of the group `sudo` are granted with sudo privileges while on **RedHat** based distributions like **CentOS** and **Fedora**, the name of the sudo group is `wheel`.

On **RedHat** based distributions such as CentOS and Fedora, the name of the sudo group is `wheel`. To add the user to the group run:

```bash
# usermod -aG wheel username
```

On Debian, Ubuntu and their derivatives, members of the group `sudo` are granted with sudo access. To add a user to the sudo group use the `usermod` command:

```bash
# usermod -aG sudo username
```

The command consists of the following parts:

- **usermod** is the tool that modifies a user account.
- **-aG** is the option that tells the command to add the user to a specific group. The `-a` option adds a user to the group without removing it from current groups. The `-G` option states the group where to add the user. In this case, these two options always go together.
- **sudo** is the group we append to the above options. In this case, it is sudo, but it can be any other group.
- **username** is the name of the user account you want to add to the sudo group.

To verify the new Debian sudo user was added to the group, run the command:

```bash
getent group sudo
```

## 2. Test the sudo access


Switch to the newly created user:

```bash
su - username
```

Use the `sudo` command to run the `whoami` command:

```bash
sudo whoami
```

If the user has sudo access then the output of the `whoami` command will be `root`.

## 3. Password Timeout

By default, sudo will ask you to enter your password again after five minutes of sudo inactivity. You can change the default timeout by editing the `sudoers` file. Open the file with `visudo`:

```bash
sudo visudo
```

Set the default timeout by adding the line below, where 10 is the timeout specified in minutes:

```txt
Defaults  timestamp_timeout=10
```

If you want to change the timestamp only for **a specific user** add the following line, where `user_name` is the user in question.

```txt
Defaults:user_name timestamp_timeout=10
```

## 4. Run a Command as a User Other than Root

There is a wrong perception that `sudo` is used only to provide root permissions to a regular user. Actually, you can use `sudo` to run a command as any user.

The `-u` option allows you to run a command as a specified user.

In the following example we are using `sudo` to run the `whoami` command as a user “richard”:

```bash
sudo -u richard whoami
```

The `whoami` command will print the name of the user running the command:

```bash
richard
```

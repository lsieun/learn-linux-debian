# How to Run Sudo Command Without Password

原文：[How to Run Sudo Command Without Password](https://linuxize.com/post/how-to-run-sudo-command-without-password/)

<!-- TOC -->

- [1. Adding User to the Sudoers File](#1-adding-user-to-the-sudoers-file)
- [2. Using `/etc/sudoers.d`](#2-using-etcsudoersd)

<!-- /TOC -->

The sudoers file contains information that determines a user’s and group’s sudo privileges.

You can configure the user sudo access by modifying the sudoers file or by adding a configuration file to the `/etc/sudoers.d` directory. The files created inside this directory will be included in the sudoers file.

## 1. Adding User to the Sudoers File

Before making any changes, it is a good idea to back up the current file:

```bash
sudo cp /etc/sudoers{,.backup_$(date +%Y%m%d)}
```

> The date command will append the current date to the backup file name.

Open the `/etc/sudoers` file with the `visudo` command:

```bash
sudo visudo
```

When making changes to the `/etc/sudoers` file always use `visudo`. This command checks the file after editing, and if there is a syntax error it will not save the changes. If you open the file with a text editor, a syntax error will result in losing the sudo access.

On most systems, the `visudo` command opens the `/etc/sudoers` file with the vim text editor.

Scroll down to the end of the file and add the following line that will allow the user “linuxize” to run any command with sudo without being asked for a password:

```txt
linuxize  ALL=(ALL) NOPASSWD:ALL
```

> Do not forget to change “linuxize” with the username you want to grant access to.

If you want to allow the user to run only specific commands without entering password specify the commands after the `NOPASSWD` keyword.

For example, to allow only the `mkdir` and `mv` commands you would use:

```txt
linuxize ALL=(ALL) NOPASSWD:/bin/mkdir,/bin/mv
```

Once done, save the file and exit the editor.

## 2. Using `/etc/sudoers.d`

Instead of editing the `sudoers` file you can create a new file with the authorization rules in the `/etc/sudoers.d` directory. This approach makes the management of the sudo privileges more maintainable.

Open your text editor and create the file:

```bash
sudo nano /etc/sudoers.d/linuxize
```

You can name the file as you want, but usually, it is a good idea to use the user name as the name of the file.

Add the same rule as you would add to the sudoers file:

```txt
linuxize  ALL=(ALL) NOPASSWD:ALL
```

Finally, save the file and close the editor.




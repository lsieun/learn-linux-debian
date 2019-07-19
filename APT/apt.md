# apt

## update

The aim of the `apt update` command is to download for each package source the corresponding `Packages` (or `Sources`) file.

## install

If the file `sources.list` mentions **several distributions**, it is possible to give **the version of the package** to install. A specific version number can be requested with `apt install package=version`, but indicating its distribution of origin (Stable, Testing or Unstable) — with `apt install package/distribution` — is usually preferred. With this command, it is possible to go back to an older version of a package (if for instance you know that it works well), provided that it is still available in one of the sources referenced by the `sources.list` file. Otherwise the [snapshot.debian.org](http://snapshot.debian.org/) archive can come to the rescue.

这段理解：

- （1） 在`sources.list`文件指定的package source中，可能包含一个package的多个版本。
- （2） 针对多个版本的问题，第一种解决方式是：`apt install <package>=<version>`
- （3） 针对多个版本的问题，第二种解决方式是：`apt install <package>/<distribution>`

```bash
## Installation of the unstable version of spamassassin
# apt install spamassassin/unstable
```

### The cache of .deb files

APT keeps a copy of each downloaded `.deb` file in the directory `/var/cache/apt/archives/`. In case of frequent updates, this directory can quickly take a lot of disk space with several versions of each package; you should regularly sort through them.

Two commands can be used: `apt-get clean` entirely empties the directory; `apt-get autoclean` only removes packages which can no longer be downloaded (because they have disappeared from the Debian mirror) and are therefore clearly useless (the configuration parameter `APT::Clean-Installed` can prevent the removal of `.deb` files that are currently installed). Note that `apt` does not support those commands.

## System Upgrade

Regular upgrades are recommended, because they include the latest security updates. To upgrade, use `apt upgrade` (of course after `apt update`).

This command looks for installed packages which can be upgraded without removing any packages. In other words, the goal is to ensure the least intrusive upgrade possible. `apt-get` is slightly more demanding than `aptitude` or `apt` because it will refuse to install packages which were not installed beforehand.

## reinstall

The system can sometimes be damaged after the removal or modification of files in a package. The easiest way to retrieve these files is to reinstall the affected package.

Unfortunately, the packaging system finds that the latter is already installed and politely refuses to reinstall it; to avoid this, use the `--reinstall` option of the `apt` and `apt-get` commands. The following command reinstalls `<package>` even if it is already present:

```bash
# apt --reinstall install <package>
```

Be careful! Using `apt --reinstall` to restore packages modified during an attack will certainly not recover the system as it was.



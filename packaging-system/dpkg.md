# dpgk

`dpkg` is the program that handles `.deb` files, notably extracting, analyzing, and unpacking them.

```bash
$ dpkg --version
Debian 'dpkg' package management program version 1.18.25 (amd64).
This is free software; see the GNU General Public License version 2 or
later for copying conditions. There is NO warranty.
```

## dpkg's database

All of **the configuration scripts** for installed packages are stored in the `/var/lib/dpkg/info/` directory, in the form of a file prefixed with the package’s name. This directory also includes a file with the .list extension for each package, containing the list of files that belong to that package.

这个目录记录scripts： `/var/lib/dpkg/info/`

```bash
$ cd /var/lib/dpkg/info/
$ ls vim*
vim-tiny.conffiles  vim-tiny.list  vim-tiny.md5sums  vim-tiny.postinst  vim-tiny.prerm
```

The `/var/lib/dpkg/status` file contains a series of data blocks (in the format of the famous mail headers, RFC 2822) describing **the status of each package**. The information from the control file of the installed packages is also replicated there.

这个文件记录status： `/var/lib/dpkg/status`

```bash
## 注意：这里是一个文件
$ cat /var/lib/dpkg/status
```

## Coexistence with Other Packaging Systems

Debian packages are not the only software packages used in the free software world. The main competitor is the **RPM** format of the Red Hat Linux distribution and its many derivatives. Red Hat is a very popular, commercial distribution. It is thus common for software provided by third parties to be offered as RPM packages rather than Debian.

In this case, you should know that the program `rpm`, which handles RPM packages, is available as a Debian package, so it is possible to use this package format on Debian. Care should be taken, however, to limit these manipulations to extract the information from a package or to verify its integrity. It is, in truth, unreasonable to use `rpm` to install an RPM on a Debian system; RPM uses its own database, separate from those of native software (such as dpkg ). This is why it is not possible to ensure a stable coexistence of two packaging systems.

On the other hand, the `alien` utility can convert **RPM packages** into **Debian packages**, and vice versa.

```bash
$ fakeroot alien --to-deb phpMyAdmin-2.0.5-2.noarch.rpm
phpmyadmin_2.0.5-2_all.deb generated
$ ls -s phpmyadmin_2.0.5-2_all.deb
64 phpmyadmin_2.0.5-2_all.deb
```

You will find that this process is extremely simple. You must know, however, that the package generated does not have any dependency information, since the dependencies in the two packaging formats don’t have systematic correspondence. The administrator must thus manually ensure that the converted package will function correctly, and this is why Debian packages thus generated should be avoided as much as possible.

Fortunately, Debian has the largest collection of software packages of all distributions, and it is likely that whatever you seek is already in there.

The stability of the software deployed using the `dpkg` tool contributes to Debian’s fame.





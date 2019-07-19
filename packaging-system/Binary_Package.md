# Binary Package

<!-- TOC -->

- [1. Structure of a Binary Package](#1-Structure-of-a-Binary-Package)
- [2. Package Meta-Information](#2-Package-Meta-Information)
  - [2.1. Description: the control File](#21-Description-the-control-File)
    - [2.1.1. Dependencies: the Depends Field](#211-Dependencies-the-Depends-Field)
    - [2.1.2. Conflicts: the Conflicts field](#212-Conflicts-the-Conflicts-field)
    - [2.1.3. Incompatibilities: the Breaks Field](#213-Incompatibilities-the-Breaks-Field)
    - [2.1.4. Provided Items: the Provides Field](#214-Provided-Items-the-Provides-Field)
    - [2.1.5. Replacing Files: The Replaces Field](#215-Replacing-Files-The-Replaces-Field)
  - [Configuration Scripts](#Configuration-Scripts)
  - [Checksums, List of Configuration Files](#Checksums-List-of-Configuration-Files)

<!-- /TOC -->

## 1. Structure of a Binary Package

The Debian package format is designed so that its content may be extracted on any Unix system that has the classic commands `ar`, `tar`, and `gzip` (sometimes `xz` or `bzip2`). This seemingly trivial property is important for portability and disaster recovery.

This is why it is easy to restore `dpkg` in the event of an erroneous deletion. You would only have to download the Debian package and extract the content from the data.tar.gz archive in the system’s root ( / ):

Dpkg Download for Linux

```txt
https://pkgs.org/download/dpkg
```

Have a look at the content of a `.deb` file:

```bash
## download the Debian package
$ wget http://ftp.br.debian.org/debian/pool/main/d/dpkg/dpkg_1.18.25_amd64.deb
## displays the list of files contained in such an archive
$ ar t dpkg_1.18.25_amd64.deb
debian-binary
control.tar.gz
data.tar.xz
## extracts the files from the archive into the current working directory
$ ar x dpkg_1.18.25_amd64.deb
## 查看解压后的内容
$ ls
control.tar.gz  data.tar.xz  debian-binary  dpkg_1.18.25_amd64.deb
$ cat debian-binary
2.0
## 解压.gz文件
$ tar -tzf control.tar.gz
./
./conffiles
./control
./md5sums
./postinst
./postrm
./prerm
## 解压.xz文件
$ tar tJf data.tar.xz | head -n 15
./
./etc/
./etc/alternatives/
./etc/alternatives/README
./etc/cron.daily/
./etc/cron.daily/dpkg
./etc/dpkg/
./etc/dpkg/dpkg.cfg
./etc/dpkg/dpkg.cfg.d/
./etc/logrotate.d/
./etc/logrotate.d/dpkg
./sbin/
./sbin/start-stop-daemon
./usr/
./usr/bin/
```

As you can see, the `ar` archive of a Debian package is comprised of **three files**:

```bash
$ ar t dpkg_1.18.25_amd64.deb
debian-binary
control.tar.gz
data.tar.xz
```

- `debian-binary`: This is a text file which simply indicates **the version** of the `.deb` file used (in 2015: version 2.0).
- `control.tar.gz`: This archive file contains **all of the available meta-information**, like the name and version of the package. Some of this meta-information allows **package management tools** to determine if it is possible to install or uninstall it, for example according to the list of packages already on the machine.
- `data.tar.xz`: This archive contains **all of the files to be extracted from the package**; this is where the executable files, documentation, etc., are all stored. Some packages may use other compression formats, in which case the file will be named differently (`data.tar.gz` for `gzip`, `data.tar.bz2` for `bzip2`, `data.tar.xz` for `XZ`).

## 2. Package Meta-Information

The Debian package is not only **an archive of files intended for installation**. It is part of a larger whole, and it describes its **relationship with other Debian packages** (dependencies, conflicts, suggestions). It also provides **scripts** that enable the execution of commands at **different stages in the package’s lifecycle** (installation, removal, upgrades). These data are used by **the package management tools** but are not part of the packaged software; they are, within the package, what is called its “meta-information” (information about other information).

这段理解：

- （1） Debian package = an archive of files + meta-information
- （2） meta-information = relationships(dependencies/conflicts/suggestions) + scripts
- （3） meta-information是由package management tools使用的，而不是由package本身使用的。
- （4） 这些meta-information就存储在`control.tar.gz`中

树状结构：

- Debian package
  - an archive of files （实际的安装文件）
  - meta-information （元信息，由package management tools使用）
    - relationships(dependencies/conflicts/suggestions)
    - scripts

再来查看一下`control.tar.gz`解压之后的内容：

```bash
$ mkdir control_dir
$ tar -zxf control.tar.gz -C control_dir/
$ cd control_dir/ && ls
conffiles  control  md5sums  postinst  postrm  prerm
```

### 2.1. Description: the control File

查看`control_dir/`目录下`control`的内容

```bash
$ cat control
Package: dpkg
Version: 1.18.25
Architecture: amd64
Essential: yes
Origin: debian
... ...
```

另外，使用命令`apt-cache show <package>`显示的内容与`control`文件显示的内容大体上相同：

```bash
## 当前的apt是1.4.8版本
$ apt --version
apt 1.4.8 (amd64)
## 在使用apt-cache show命令时，查询到了新版本（1.4.9）的信息
$ apt-cache show apt
Package: apt
Version: 1.4.9
Installed-Size: 3539
Maintainer: APT Development Team <deity@lists.debian.org>
Architecture: amd64
Replaces: apt-utils (<< 1.3~exp2~)
Depends: adduser, gpgv | gpgv2 | gpgv1, debian-archive-keyring, init-system-helpers (>= 1.18~), libapt-pkg5.0 (>= 1.3~rc2), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2)
Recommends: gnupg | gnupg2 | gnupg1
Suggests: apt-doc, aptitude | synaptic | wajig, dpkg-dev (>= 1.17.2), powermgmt-base, python-apt
Breaks: apt-utils (<< 1.3~exp2~)
Description-en: commandline package manager
 This package provides commandline tools for searching and
 managing as well as querying information about packages
 as a low-level access to all features of the libapt-pkg library.
 .
 These include:
  * apt-get for retrieval of packages and information about them
    from authenticated sources and for installation, upgrade and
    removal of packages together with their dependencies
  * apt-cache for querying available information about installed
    as well as installable packages
  * apt-cdrom to use removable media as a source for packages
  * apt-config as an interface to the configuration settings
  * apt-key as an interface to manage authentication keys
Description-md5: 9fb97a88cb7383934ef963352b53b4a7
Tag: admin::package-management, devel::lang:ruby, hardware::storage,
 hardware::storage:cd, implemented-in::c++, implemented-in::perl,
 implemented-in::ruby, interface::commandline, network::client,
 protocol::ftp, protocol::http, protocol::ipv6, role::program,
 scope::application, scope::utility, sound::player, suite::debian,
 use::downloading, use::organizing, use::searching, works-with::audio,
 works-with::software:package, works-with::text
Section: admin
Priority: important
Filename: pool/main/a/apt/apt_1.4.9_amd64.deb
Size: 1231594
MD5sum: 5e3b6c66100964833307d3a943918275
SHA256: dddf4ff686845b82c6c778a70f1f607d0bb9f8aa43f2fb7983db4ff1a55f5fae

Package: apt
Status: install ok installed
Priority: important
Section: admin
Installed-Size: 3539
Maintainer: APT Development Team <deity@lists.debian.org>
Architecture: amd64
Version: 1.4.8
Replaces: apt-utils (<< 1.3~exp2~)
Depends: adduser, gpgv | gpgv2 | gpgv1, debian-archive-keyring, init-system-helpers (>= 1.18~), libapt-pkg5.0 (>= 1.3~rc2), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2)
Recommends: gnupg | gnupg2 | gnupg1
Suggests: apt-doc, aptitude | synaptic | wajig, dpkg-dev (>= 1.17.2), powermgmt-base, python-apt
Breaks: apt-utils (<< 1.3~exp2~)
Conffiles:
 /etc/apt/apt.conf.d/01autoremove 0b1391c01d75f95fa4ea5ac01219b515
 /etc/cron.daily/apt-compat bc4a71cbcaeed4179f25d798257fa980
 /etc/kernel/postinst.d/apt-auto-removal 4ad976a68f045517cf4696cec7b8aa3a
 /etc/logrotate.d/apt 179f2ed4f85cbaca12fa3d69c2a4a1c3
Description-en: commandline package manager
 This package provides commandline tools for searching and
 managing as well as querying information about packages
 as a low-level access to all features of the libapt-pkg library.
 .
 These include:
  * apt-get for retrieval of packages and information about them
    from authenticated sources and for installation, upgrade and
    removal of packages together with their dependencies
  * apt-cache for querying available information about installed
    as well as installable packages
  * apt-cdrom to use removable media as a source for packages
  * apt-config as an interface to the configuration settings
  * apt-key as an interface to manage authentication keys
Description-md5: 9fb97a88cb7383934ef963352b53b4a7
```

#### 2.1.1. Dependencies: the Depends Field

The dependencies are defined in the `Depends` field in the package header.

```txt
Depends: adduser, gpgv | gpgv2 | gpgv1, debian-archive-keyring, init-system-helpers (>= 1.18~), libapt-pkg5.0 (>= 1.3~rc2), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2)
```

This is **a list of conditions** to be met for the package to work correctly — this information is used by tools such as `apt` in order to install the required libraries, in appropriate versions fulfilling the dependencies of the package to be installed.

从“整体”角度来说：a list of conditions

**For each dependency, it is possible to restrict the range of versions that meet that condition**. In other words, it is possible to express the fact that we need the package `libc6` in a version equal to or greater than “2.15” (written “ libc6 (>=2.15) ”).

从“个体”角度来说：each dependency

Version comparison operators are as follows:

- `<<`: less than
- `<=`: less than or equal to
- `=`: equal to (note that “ 2.6.1 ” is not equal to “ 2.6.1-1 ”)
- `>=`: greater than or equal to
- `>>`: greater than

复制，方便查看：

```txt
Depends: adduser, gpgv | gpgv2 | gpgv1, debian-archive-keyring, init-system-helpers (>= 1.18~), libapt-pkg5.0 (>= 1.3~rc2), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2)
```

In a list of conditions to be met, **the comma** serves as a separator. It must be interpreted as a logical “and”. In conditions, the **vertical bar** (“|”) expresses a logical “or” (it is an inclusive “or”, not an exclusive “either/or”). Carrying greater priority than “and”, it can be used as many times as necessary. Thus, the dependency “(A or B) and C” is written `A | B, C`. In contrast, the expression “A or (B and C)” should be written as “(A or B) and (A or C)”, since the `Depends` field does not tolerate parentheses that change the order of priorities between the logical operators “or” and “and”. It would thus be written `A | B, A | C`.

联想：每一个dependency是一个“珠子”，而这里的“逻辑关系”就是一根“绳子”，把那些“珠子”串起来。

The **dependencies system** is a good mechanism for guaranteeing the operation of a program, but it has another use with “**meta-packages**”. These are **empty packages** that only describe dependencies. They facilitate the installation of a consistent group of programs preselected by the meta-package maintainer; as such, `apt install meta-package` will automatically install all of these programs using the meta-package’s dependencies. The `gnome`, `kde-full` and `linux-image-amd64` packages are examples of meta-packages.

这段理解：

- （1） 这种dependencies system又产生另外一种使用场景，那就是meta-packages。
- （2） meta-packages是empty packages，其中只描述了dependencies，为其他的软件安装做准备

```txt
Depends: adduser, gpgv | gpgv2 | gpgv1, debian-archive-keyring, init-system-helpers (>= 1.18~), libapt-pkg5.0 (>= 1.3~rc2), libc6 (>= 2.15), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2)
Recommends: gnupg | gnupg2 | gnupg1
Suggests: apt-doc, aptitude | synaptic | wajig, dpkg-dev (>= 1.17.2), powermgmt-base, python-apt
```

The `Recommends` and `Suggests` fields describe dependencies that are not compulsory. The “recommended” dependencies, the most important, considerably improve the functionality offered by the package but are not indispensable to its operation. The “suggested” dependencies, of secondary importance, indicate that certain packages may complement and increase their respective utility, but it is perfectly reasonable to install one without the others.

- indispensable
  - 不可或缺的；必不可少的 too important to be without

You should always install the “recommended” packages, unless you know exactly why you do not need them. Conversely, it is not necessary to install “suggested” packages unless you know why you need them.

The `Enhances` field also describes a suggestion, but in a different context. It is indeed located in the suggested package, and not in the package that benefits from the suggestion. Its interest lies in that it is possible to add a suggestion without having to modify the package that is concerned. Thus, all add-ons, plug-ins, and other extensions of a program can then appear in the list of suggestions related to the software. Although it has existed for several years, **this last field is still largely ignored** by programs such as `apt` or `synaptic` . Its purpose is for a suggestion made by the `Enhances` field to appear to the user in addition to the traditional suggestions — found in the `Suggests` field.

这段没有看懂。

#### 2.1.2. Conflicts: the Conflicts field

The `Conflicts` field indicates when a package cannot be installed simultaneously with another.

The most common reasons for this are that both packages include a file of the same name, or provide the same service on the same TCP port, or would hinder each other’s operation.

举例说明

`dpkg` will refuse to install a package if it triggers a conflict with an already installed package, except if the new package specifies that it will “replace” the installed package, in which case `dpkg` will choose to replace the old package with the new one.

`apt` always follows your instructions: if you choose to install a new package, it will automatically offer to uninstall the package that poses a problem.

#### 2.1.3. Incompatibilities: the Breaks Field

```txt
Breaks: apt-utils (<< 1.3~exp2~)
```

The `Breaks` field has an effect similar to that of the `Conflicts` field, but with a special meaning. It signals that the installation of a package will “break” another package (or particular versions of it). In general, this incompatibility between two packages is transitory, and the `Breaks` relationship specifically refers to the incompatible versions.

`dpkg` will refuse to install a package that breaks an already installed package, and `apt` will try to resolve the problem by updating the package that would be broken to a newer version (which is assumed to be fixed and, thus, compatible again).

This type of situation may occur in the case of **updates without backwards compatibility**: this is the case if the new version no longer functions with the older version, and causes a malfunction in another program without making special provisions. The `Breaks` field prevents the user from running into these problems.

#### 2.1.4. Provided Items: the Provides Field

This field introduces the very interesting concept of a “**virtual package**”. It has many roles, but **two are of particular importance**. The first role consists in **using a virtual package to associate a generic service with it** (the package “provides” the service). The second indicates that **a package completely replaces another**, and that for this purpose it can also satisfy the dependencies that the other would satisfy. It is thus possible to create a substitution package without having to use the same package name.

#### 2.1.5. Replacing Files: The Replaces Field

The `Replaces` field indicates that the package contains files that are also present in another package, but that the package is legitimately entitled to replace them.

Without this specification, `dpkg` fails, stating that it can not overwrite the files of another package (technically, it is possible to force it to do so with the `--force-overwrite` option, but that is not considered standard operation). This allows identification of potential problems and requires the maintainer to study the matter prior to choosing whether to add such a field.

The use of this field is justified when package names change or when a package is included in another. This also happens when the maintainer decides to distribute files differently among various binary packages produced from the same source package: a replaced file no longer belongs to the old package, but only to the new one.

If all of the files in an installed package have been replaced, the package is considered to be removed.

Finally, this field also encourages `dpkg` to remove the replaced package where there is a conflict.

```txt
Tag: admin::package-management, devel::lang:ruby, hardware::storage,
 hardware::storage:cd, implemented-in::c++, implemented-in::perl,
 implemented-in::ruby, interface::commandline, network::client,
 protocol::ftp, protocol::http, protocol::ipv6, role::program,
 scope::application, scope::utility, sound::player, suite::debian,
 use::downloading, use::organizing, use::searching, works-with::audio,
 works-with::software:package, works-with::text
```

In the `apt` example above, we can see the presence of a field that we have not yet described, the `Tag` field. This field does not describe a relationship between packages, but is simply a way of **categorizing a package in a thematic taxonomy**. This classification of packages according to several criteria (type of interface, programming language, domain of application, etc.) has been available for a long time. Despite this, not all packages have accurate tags and it is not yet integrated in all Debian tools; `aptitude` displays these tags, and allows them to be used as search criteria.

### Configuration Scripts

In addition to the `control` file, the `control.tar.gz` archive for each Debian package may contain **a number of scripts**, called by `dpkg` at different stages in the processing of a package.

这段理解：`control.tar.gz`除了包含`control`文件，还包含a number of scripts。

```bash
$ tar -tzf control.tar.gz
./
./conffiles
./control
./md5sums
./postinst    # 这个是scripts
./postrm      # 这个是scripts
./prerm       # 这个是scripts
```

The **Debian Policy** describes the possible cases in detail, specifying the scripts called and the arguments that they receive. These sequences may be complicated, since if one of the scripts fails, `dpkg` will try to return to a satisfactory state by canceling the installation or removal in progress (insofar as it is possible).

这段理解：

- （1） Debian Policy描述了很多使用情景，例如：要使用哪个script，要传递哪些arguments
- （2） 如果script安装失败了，`dpkg`会尽量退还到a satisfactory state

In general, the `preinst` script is executed prior to installation of the package, while the `postinst` follows it. Likewise, `prerm` is invoked before removal of a package and `postrm` afterwards.

这段理解：介绍这几个script的作用

An update of a package is equivalent to removal of the previous version and installation of the new one. It is not possible to describe in detail all the possible scenarios here, but we will discuss the most common two: **an installation/update** and **a removal**.

### Checksums, List of Configuration Files

In addition to the **maintainer scripts** and `control` data already mentioned in the previous sections, the `control.tar.gz` archive of a Debian package may contain **other interesting files**.

```bash
$ tar -tzf control.tar.gz
./
./conffiles    # 这个是interesting file
./control
./md5sums      # 这个是interesting file
./postinst
./postrm
./prerm
```

The first, `md5sums`, contains the MD5 checksums for all of the package’s files. Its main advantage is that it allows `dpkg --verify` to check if these files have been modified since their installation. Note that when this file doesn’t exist, dpkg will generate it dynamically at installation time (and store it in the dpkg database just like other control files).

`conffiles` lists package files that must be handled as **configuration files**. Configuration files can be modified by the administrator, and `dpkg` will try to preserve those changes during a package update.


# APT

What makes Debian so popular with administrators is **how easily software can be installed** and **how easily the whole system can be updated**. This unique advantage is largely due to the `APT` program.

`APT` is the abbreviation for **Advanced Package Tool**. What makes this program “advanced” is its approach to packages. It doesn’t simply evaluate them individually, but it considers them as a whole and produces the best possible combination of packages depending on what is available and compatible (according to dependencies).

`APT` is a group of programs that allows the execution of higher-level modifications to the system: installing or removing a package (while keeping dependencies satisfied), updating the system, listing the available packages, etc.

> The word `source` can be ambiguous. A **source package** — a package containing the source code of a program — should not be confused with a **package source** — a repository (website, FTP server, CD-ROM, local directory, etc.) which contains packages.

APT needs to be given a “list of package sources”: the file `/etc/apt/sources.list` will list the different repositories (or “sources”) that publish Debian packages. APT will then import **the list of packages** published by each of these sources. This operation is achieved by downloading `Packages.xz` or a variant using a different compression method (such as `Packages.gz` or `.bz2`) files (in case of a source of binary packages) and `Sources.xz` or a variant (in case of a source of source packages) and by analyzing their contents. When an old copy of these files is already present, APT can update it by only downloading the differences.

If many package sources are referenced, it can be useful to split them in multiple files. Each part is then stored in `/etc/apt/sources.list.d/filename.list`.

## Filling in the sources.list File

### Syntax

Each active line of the `/etc/apt/sources.list` file contains the description of a source, made of **3 parts** separated by **spaces**.

```txt
deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib
deb http://mirrors.aliyun.com/debian-security stretch/updates main
deb-src http://mirrors.aliyun.com/debian-security stretch/updates main
deb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
deb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
```

**The first field** indicates the source type:

- “deb” for binary packages,
- “ deb-src ” for source packages.

**The second field** gives **the base URL of the source** (combined with the filenames present in the `Packages.gz` files, it must give a full and valid URL): this can consist in a Debian mirror or in any other package archive set up by a third party. The URL can start with `file://` to indicate a local source installed in the system’s file hierarchy, with `http://` to indicate a source accessible from a web server, or with `ftp://` for a source available on an FTP server. The URL can also start with `cdrom:` for CD-ROM/DVD-ROM/Blu-ray disc based installations, although this is less frequent, since network-based installation methods are more and more common.

The syntax of **the last field** depends on the **structure of the repository**. In the simplest cases, you can simply indicate a subdirectory (with a required trailing slash) of the desired source (this is often a simple “ `./`” which refers to the absence of a subdirectory — the packages are then directly at the specified URL). But in the most common case, the repositories will be structured like a **Debian mirror**, with multiple distributions each having multiple components.

Debian uses **three sections** to **differentiate packages** according to the licenses chosen by the authors of each work.

- main
- contrib
- non-free

`Main` gathers all packages which fully comply with the Debian Free Software Guidelines.

The `non-free` archive is different because it contains software which does not (entirely) conform to these principles but which can nevertheless be distributed without restrictions. This archive, which is not officially part of Debian, is a service for users who could need some of those programs — however Debian always recommends giving priority to free software. The existence of this section represents a considerable problem for Richard M. Stallman and keeps the Free Software Foundation from recommending Debian to users.

`Contrib` (contributions) is a set of open source software which cannot function without some non-free elements. These elements can be software from the non-free section, or non-free files such as game ROMs, BIOS of consoles, etc. `Contrib` also includes free software whose compilation requires proprietary elements. This was initially the case for the OpenOffice.org office suite, which used to require a proprietary Java environment.

### Repositories for Stable Users

Here is a standard `sources.list` for a system running the **Stable** version of Debian:

```bash
# Security updates
deb http://security.debian.org/ jessie/updates main contrib non-free
deb-src http://security.debian.org/ jessie/updates main contrib non-free

## Debian mirror

# Base repository
deb http://ftp.debian.org/debian jessie main contrib non-free
deb-src http://ftp.debian.org/debian jessie main contrib non-free

# Stable updates
deb http://ftp.debian.org/debian jessie-updates main contrib non-free
deb-src http://ftp.debian.org/debian jessie-updates main contrib non-free

# Stable backports
deb http://ftp.debian.org/debian jessie-backports main contrib non-free
deb-src http://ftp.debian.org/debian jessie-backports main contrib non-free
```

This file lists all sources of packages associated with the `Jessie` version of Debian (the current Stable as of this writing). We opted to name “`jessie`” explicitly instead of using the corresponding “stable“ alias (`stable` , `stable-updates` , `stable-backports` ) because we don’t want to have the underlying distribution changed outside of our control when the next stable release comes out.

Most packages will come from the “base repository” which contains all packages but is seldom updated (about once every 2 months for a “point release”). The other repositories are partial (they do not contain all packages) and can host updates (packages with newer version) that APT might install. The following sections will explain the purpose and the rules governing each of those repositories.

Note that when the desired version of a package is available on several repositories, the first one listed in the sources.list file will be used. For this reason, **non-official sources are usually added at the end of the file**.

#### Security Updates

The security updates are not hosted on the usual network of Debian mirrors but on security.debian.org (on a small set of machines maintained by the Debian System Administrators). This archive contains **security updates** (prepared by the Debian Security Team and/or by package maintainers) for the **Stable distribution**.

#### Stable Updates

### apt Commands



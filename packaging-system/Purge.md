# Purge

When a Debian package is removed, the **configuration files** are retained in order to facilitate possible re-installation. Likewise, **the data generated** by a daemon (such as the content of an LDAP server directory, or the content of a database for an SQL server) are usually retained.

To remove all data associated with a package, it is necessary to “purge” the package with the command, `dpkg -P package`, `apt-get remove --purge package` or `aptitude purge package`.

Given the definitive nature of such data removals, a purge should not be taken lightly.

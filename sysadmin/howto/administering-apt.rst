HOWTO: Administering APT
########################

Various tips and tricks for administering APT on Debian/Ubuntu servers

--------

General information about any package, installed or otherwise, including version
and description::

    apt-cache show <pkgname>

Summary (name, version, arch) for all **installed** packages matching
*pkgname*::

    dpkg -l <pkgname>

List installed packages::

    dpkg --get-selections

List the files installed by a specified package::

    dpkg -L <pkgname>

Find out which package provides ``/usr/bin/foo``::

    dpkg -S /usr/bin/foo


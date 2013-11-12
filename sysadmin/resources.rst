Some useful sysadmin resources for OKFN services:

-  Our mercurial sysadmin repository for scripts, common config etc:
   https://bitbucket.org/okfn/sysadmin/

   -  in particular our
      `fabfile <https://bitbucket.org/okfn/sysadmin/src/default/bin/fabfile.py>`__
      to customize newly deployed servers. *cd* into its containing
      directory and run *fab -h* and *fab -d {command}*. Example: ''fab
      ssh\_add\_public\_key\_group:../ssh\_key.js,sysadmin,ubuntu --user
      ubuntu --host {host}

''

-  The `OKFN trac <http://trac.okfn.org/>`__, e.g. its `list of active
   tickets <http://trac.okfn.org/query?status=new&status=assigned&status=reopened&component=sysadmin&order=priority>`__
-  The `overview <Sysadmin/Services>`__ of OKFN services and servers

Useful resources on Ubuntu/Debian packaging:

-  `Ubuntu packaging
   guide <https://wiki.ubuntu.com/PackagingGuide/Complete>`__
-  `Ubuntu packaging guide
   Python <https://wiki.ubuntu.com/PackagingGuide/Python>`__
-  `Debian Python
   Policy <http://www.debian.org/doc/packaging-manuals/python-policy/>`__
   includes bits about packaging Python modules and programs
-  Debian `APT
   HowTo <http://www.debian.org/doc/manuals/apt-howto/ch-apt-get.en.html#s-pin>`__
   on *Pinning*
-  StackOverflow *`How to build a Debian/Ubuntu package from
   source? <http://stackoverflow.com/questions/130894/how-to-build-a-debian-ubuntu-package-from-source>`__*
-  `update-alternatives <http://linux.die.net/man/8/update-alternatives>`__
   man page (helpful when installing several versions in parallel)

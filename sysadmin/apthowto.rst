Listing Information About Packages
==================================

-  general information about the package including version and
   description

| ``apt-cache show ``\ \ `` ``
| ``dpkg -l '``\ \ ``' -- see [1]     ``

-  list installed packages:

``dpkg -l pattern | grep '^i'     * dpkg --status package     ``

-  To list the files installed from a specified package, issue the
   command:

``dpkg --listfiles package  ``

Biblio
------

-  http://www.oreilly.com/catalog/debian/chapter/book/appc_03.html

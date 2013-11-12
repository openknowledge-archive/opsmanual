This article describes how to deploy code to OKFN's Wordpress blogfarm
at WPEngine. See `Sysadmin/Blogfarm <Sysadmin/Blogfarm>`__ for general
information about the OKFN blogfarm

General
-------

Some basic instructions outlining the code deployment procedures for our
blog farm hoster WPE (WP Engine - http://wpengine.com/).

WPE code deployment is done through their git post-recieve hook. In
essence it's as simply as commiting code changes to the repository and
doing a git push back to the WPE remote. More detailed instructions can
be found in their documentation about this - http://git.wpengine.com/.

Access to the WPE git repos is controlled by public keys. You'll need to
contact the OKF Sysadmin team to have your public key granted access
before proceeding. Once this has been done, confirm access by:

::

    $ ssh git@git.wpengine.com info

You should get a response back listing the repositories you have access
to. At a minimum these should be:

::

    production/okf
    staging/okf

Consider these as two separate git remotes containing the code for the
production web dir and staging web dir respectively. For now, disregard
the staging remote as we don't have anything setup there.

The first thing you want to do is sync with the production remote,
depending on how you have your local code setup you can add this remote
to an existing repo, or create a new working directory and run the
following:

::

    $ git clone git@git.wpengine.com:production/okf.git .

This repository uses submodules for the various plugins and themes. So
before doing anything else you should initialize and update these.

::

    $ git submodule init
    $ git submodule update

Deploying changes is as simple as pushing your commits back to the
production remote. So something like the following. It's advised that
you do this via a terminal window so that you can see descriptive
deployment progress output, along with any custom errors.

::

    $ git push origin master

Or run git remote -v to list the remotes of your local repo. If you
cloned from the production repo then that is most like the "origin"
remote.

As always, be sure to retrieve changes made by other users before
pushing back to WPE.

::

    $ git pull

Sub Modules
-----------

Note that submodules are referenced to a master repository via commit
ID's, rather than branches. As such, changes to submodules need to be
committed to the master repo as well. That is, if you commit a change to
a submodule, the latest commit for that repository has progressed, and
that new status needs to be reflected in the master repo. If you make
changes to the submodule outside of the main repo working directory,
you'll need to git pull that submodule into the main working dir and
commit it's new pointer to that main repo.

When you deploy to WPE, for any submodule that the master repository
knows about, the commit for that submodule must be accessible to WPE.
This means 2 things: 1) The repo must be publicly readable. WPE does not
provide a public key for you to grant access to private repos. 2) The
commit to retrieve must be available on that remote (pushed)

The latter is often forgotten. Use the following one liner to push all
submodules from your master repo root dir.

::

    $ git submodule foreach git push master origin

It's typically easier to work on all files (main repo and submodules)
within the same working directory so that you can commit to both the sub
and main repos quickly.

If other users make changes to the sub-modules, you'll need to retrieve
these to your own local dir also. So before making any changes, it's
good practice to do the following:

::

    $ git pull origin master
    $ git submodule foreach git pull origin master

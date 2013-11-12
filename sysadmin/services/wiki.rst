Wiki "farm"
###########

This article is about our MediaWiki-based wikifarm on
wikifarm.okserver.org.

**Note we are about to merge all existing MediaWiki-based wikis into the
central wikifarm. Please see http://trac.okfn.org/ticket/979 for the
current situation.**

Technical details
-----------------

-  Installed on wikifarm.okserver.org (that is currently
   s013.okserver.org at EC2) in /home/okfn/var/wikifarm/
-  It is currently using the central mysql service on mysql-int.okfn.org
   (s005.okserver.org at EC2).
-  Currently all wiki's are cached at
   `cache.okserver.org <http://trac.okfn.org/wiki/CacheService>`__ (that
   is currently s006.okserver.org)
-  It is based on MediaWiki (currently 1.19.2)
-  The wiki farming based on a php config-switch as described
   `here <http://www.mediawiki.org/wiki/Manual:Wiki_family>`__. It is
   **not** using softlinks, another popular method.
-  We serve pages as http://$domainname/$PageName (though the `Mediawiki
   manual <http://www.mediawiki.org/wiki/Manual:Short_URLs>`__
   recommends http://$domainname/wiki/$PageName), see. That is done in
   /home/okfn/var/wikifarm/.htaccess via a redirect /$PageName -->
   /wiki/index.php/$PageName. Therefore wiki instances should use these
   path settings:

| ``$wgScriptPath       = "/wiki";``
| ``$wgArticlePath      = "/$1"; ``

How create an instance in wiki farm
-----------------------------------

Prepare the database on the central DB server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On s005, create a new DB:

| ``s005:~$ mysql -u root --password=`sudo cat /etc/mysql/rootpw```
| ``mysql> CREATE DATABASE testwiki2_okserver_org;``
| ``mysql> CREATE USER testwiki2 IDENTIFIED BY 'testwikipass2';``
| ``mysql> GRANT ALL ON testwiki2_okserver_org.* TO testwiki2;``
| ``mysql> quit``

Importing an old wiki
'''''''''''''''''''''

If you have a dump, import it. You can do this from s005 or s013:

``s005:~$ cat wikidump.sql | mysql -h mysql-int.okfn.org -u testwiki2 --password=testwikipass2 testwiki2_okserver_org``

Back on the wikifarm server s013, test the new DB:

``s013:~$ echo 'show tables;' | mysql -h mysql-int.okfn.org -u testwiki2 --password=testwikipass2 testwiki2_okserver_org``

If its a new wiki setup
'''''''''''''''''''''''

**NOTE** ''This only works if the wikimedia version being setup is the
latest version. To get a table schema data for an older version, you may
use 'mysqldump -d -uuser -ppass old\_db >wiki\_schema.sql' '' Download
the sql file for mediawiki, linked under
http://www.mediawiki.org/wiki/Manual:Database_layout

`` wget '``\ ```https://gerrit.wikimedia.org/r/gitweb?p=mediawiki/core.git;a=blob_plain;f=maintenance/tables.sql;hb=HEAD`` <https://gerrit.wikimedia.org/r/gitweb?p=mediawiki/core.git;a=blob_plain;f=maintenance/tables.sql;hb=HEAD>`__\ ``' -O mediawiki.sql``

Import the .sql

``  mysql -u root --password='somepass' testwiki2_okserver_org < mediawiki.sql``

Prepare wiki configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^

On the wikifarm server s013, create a Mediawiki configuration file. You
can either use one of the existing LocalSettings-\*.php configs as
template, or bring your existing one. Make sure it has the correct
database setting you used above:

``s013:~$ vi /home/okfn/var/wikifarm/wiki/LocalSettings-testwiki2.okserver.org.php'''``

| ``$wgDBtype           = "mysql";``
| ``$wgDBserver         = "mysql-int.okfn.org";``
| ``$wgDBname           = "testwiki2_okserver_org";``
| ``$wgDBuser           = "testwiki2";``
| ``$wgDBpassword       = "testwikipass2";``
| ``$wgDBprefix         = "";``

The wiki sould use these paths:

| ``$wgScriptPath       = "/wiki";``
| ``$wgArticlePath      = "/$1"; ``

Make sure that all anti-spam extensions are activated

| ``require_once( "$IP/extensions/AntiBot/AntiBot.php" );``
| ``require_once( "$IP/extensions/AntiSpoof/AntiSpoof.php" );``
| ``require_once( "$IP/extensions/SimpleAntiSpam/SimpleAntiSpam.php" );``
| ``require_once( "$IP/extensions/TitleBlacklist/TitleBlacklist.php" );``
| ``require_once( "$IP/extensions/SpamBlacklist/SpamBlacklist.php" );``

Once you are done with the instance's configuration, invoke it from the
central farm configuration:

``s013:~$ vi /home/okfn/var/wikifarm/wiki/LocalSettings.php``

| ``case "testwiki2.okserver.org":``
| ``   require_once "LocalSettings-testwiki2.okserver.org.php";``
| ``   break;``

Now run the Mediawiki update script:

| ``s013:~$ cd /home/okfn/var/wikifarm/wiki/maintenance/``
| ``s013:~$ php update.php --conf /home/okfn/var/wikifarm/wiki/LocalSettings-testwiki2.okserver.org.php``

If it fails, you have to fix the
LocalSettings-testwiki2.okserver.org.php. E.g. some extensions might not
be present or have other names now.

Prepare Apache configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before we touch the Apache configuration we should make sure the local
clone of the sysadmin repo is up2date:

| ``s013:~$ cd /home/okfn/hg-sysadmin/``
| ``s013:~$ hg pull ``
| ``s013:~$ hg update``
| ``s013:~$ hg stat``

If the last command shows any changes, check them in before you proceed.

Now tell Apache about the new wiki instance

``s013:~$ vi /etc/apache2/sites-available/testwiki.okserver.org``

``ServerAlias    testwiki2.okserver.org``

If the syntax check does not show any errors, reload Apache:

| ``s013:~$ sudo apache2ctl -S``
| ``s013:~$ sudo /etc/init.d/apache2 reload``

**Warning**: If you need to put in wiki-specific rewrite rules (e.g.
redirects), **make sure they are specific to your instance's domainname
and do not affect the other wiki instances**. If you use "RewriteCond",
don't forget that you have to prefix each of your rules seperately

| ``RewriteCond %{HTTP_HOST} ^testwiki2.okserver.org$``
| ``RewriteRule    ^/foo1\.html$  bar.html  [R]``
| ``RewriteCond %{HTTP_HOST} ^testwiki2.okserver.org$``
| ``RewriteRule    ^/foo2\.html$  baz.html  [R]``

Test
^^^^

If the DNS record in question does not yet exist, you can test with with
this line in your local /etc/hosts (do not forget to remove afterwards!)

``mycomputer:~$ sudo vi /etc/hosts``

``46.51.142.120 testwiki2.okserver.org``

Now point open your new wiki instance in your browser and verify it is
working as expected:

-  Main page: http://testwiki2.okserver.org/
-  Version & extensions: http://testwiki2.okserver.org/Special:Version

Set DNS records
^^^^^^^^^^^^^^^

If you do not want the new instance to be cached, set this DNS record
(see `Sysadmin/DomainServices <Sysadmin/DomainServices>`__):

-  testwiki2.okserver.org. IN CNAME wikifarm.okserver.org.

If you do want it to be cached, configure the squid cache as per
`documentation <http://trac.okfn.org/wiki/CacheService>`__ and set this
DNS record:

-  testwiki2.okserver.org. IN CNAME cache.okserver.org.

Old wiki installations
----------------------

A number of wikis on s013:/home/okfn/var/ are not yet migrated into the
wiki farm, see http://trac.okfn.org/ticket/979 for the current status.

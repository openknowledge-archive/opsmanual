Wikifarm
########

This article is about our MediaWiki-based wikifarm.

The Mediawiki instance hosts multiple wiki sites, its based on: http://www.mediawiki.org/wiki/Manual:Wiki_family#Scenario_2:_Quick_set-up

Add a new wiki site to the farm
###############################

1. Create a new mysql db for the new site with the name wiki_<domain>

::

 #mysql -u root -p
 mysql> CREATE DATABASE wiki_<domain>;
 mysql> CREATE USER wiki_<domain> IDENTIFIED BY <pass>;
 mysql> GRANT ALL ON wiki_<domain>.* TO wiki_<domain>;
 mysql> quit


New Wiki site configuration
###########################


* On the wikifarm host, we create a new configuration file for the site.

* You can use the existing LocalSettings-wiki.okfn.org.php config as a template. 

* Ideally set the name of the config file as LocalSettings-<domain>.php.

1. Update the db credentials we setup in the previous step.

:: 

 # vi /var/www/wiki/LocalSettings-<domain>.php
 
 $wgDBtype           = "mysql";
 $wgDBserver         = "localhost";
 $wgDBname           = "wiki_dm2e_eu";
 $wgDBuser           = "wiki_dm2e_eu";
 $wgDBpassword       = "w1k1dm2e";

2. Make sure that all required extensions similar to wiki.okfn.org are enabled.

::

 require_once( "$IP/extensions/AntiBot/AntiBot.php" );
 require_once( "$IP/extensions/AntiSpoof/AntiSpoof.php" );
 require_once( "$IP/extensions/SimpleAntiSpam/SimpleAntiSpam.php" );
 require_once( "$IP/extensions/TitleBlacklist/TitleBlacklist.php" );
 require_once( "$IP/extensions/SpamBlacklist/SpamBlacklist.php" );

3. Once you are done with the instance's configuration file set it up to be accessible - add a case statement for the new site.

::

 vi /var/www/wikifarm/wiki/LocalSettings.php

 case "<wiki domain>.name":
  require_once LocalSettings-<domain>.php;
  break;

4. Run the Mediawiki update script:

::
 
 cd /var/www/wikifarm/wiki/maintenance
 php update.php --conf /var/www/wikifarm/wiki/LocalSettings-<domain>.php

If update.php fails, LocalSettings-<domain>.php might need fixing - some extensions might not be present or have other names.

Setup DNS records
#################

* Dns records for the domain will need to be updated to point to this host.


Nginx config
###################

* Once the wiki instance is setup, the nginx config which is maintained in ansible for the wiki host will need to be updated.




Installed Required Modules
==========================

-  Assumes you already have apache2 (e.g. apache2-mpm-worker) installed.

| ``apt-get install php5-cgi libapache2-mod-fastcgi``
| ``# if you want mysql access (e.g. for wordpress ...)``
| ``apt-get install php5-mysql``

-  In Ubuntu libapache2-mod-fastcgi requires enabling Multiverse
   repository in /etc/apt/sources.list

   -  You'll also need to ensure the actions module is enabled:

| ``a2enmod actions fastcgi``
| ``# for mod rewrite (for wordpress ...)``
| ``a2enmod rewrite``

-  Set up fastcgi wrapper script and install at /etc/apache2/fastcgi-php
   (you can change this if you wish but remember to change apache config
   below):

| ``#!/bin/sh``
| ``PHPRC="/etc/php5/cgi/php.ini"``
| ``export PHPRC``
| ``PHP_FCGI_CHILDREN=4``
| ``export PHP_FCGI_CHILDREN``
| ``exec /usr/bin/php5-cgi``

-  Make sure the script is executable:

| ``chmod +x /etc/apache2/fastcgi-php``

-  Add to /etc/apache2/conf.d/fastcgi-php

| ``# See ``\ ```http://www.fastcgi.com/docs/faq.html#PHP`` <http://www.fastcgi.com/docs/faq.html#PHP>`__\ `` (slightly modified)``
| ``FastCgiServer /etc/apache2/fastcgi-php``
| ``ScriptAlias /cgi-bin/php /etc/apache2/fastcgi-php``
| ``AddHandler php-fastcgi .php``
|
| `` SetHandler fastcgi-script``
| `` SetOutputFilter stripcomments``
|
| ``Action php-fastcgi /cgi-bin/php``
| ``AddType application/x-httpd-php .php``

-  That's it!

About
=====

We currently run a reverse web cache and a redirector on s006 =
cache.okserver.org (aka eu6).

-  The squid cache is mainly for web services in the EC2 datacenter
   "eu-west-1". It's config it
   `squid.conf <https://bitbucket.org/okfn/sysadmin/src/default/etc/squid3/squid.conf>`__.
-  The nginx redirector is running on port 8080 and 8081. It's config is
   `/etc/nginx/sites-enabled/okfn-redirects <https://bitbucket.org/okfn/sysadmin/src/default/etc/nginx/okfn-redirects>`__.

Modifying the squid configuration
=================================

Before you modify the squid.conf, please observe the following:

-  **The cache is critical to most of our web services. Breaking the
   squid.conf means usually breaking most of our web services at once.**
-  Do not edit the actual config file /etc/squid3/squid.conf itself,
   your changes would get lost. Instead, edit the copy
   ~okfn/etc/squid3/squid.conf and then activate it with "sudo make"
   (see below).
-  Don't forget to commit and push afterwards.

To have a web service being handled by the cache, its domain names
("!ServerName", "!ServerAlias") must point to
cache.okserver.org=46.51.189.76, either as CNAME or as A record.

The procedure to modify the squid configuration is:

-  Log into s006.

Enter repository and update it:

| ``cd ~okfn/etc/squid3/``
| ``hg pull``
| ``hg update``

Modify the repo copy of squid.conf. It is pretty much self-explanatory,
see section "squid.conf" below for more information.

``sudo vi squid.conf``

There is a single command to verify correct syntax of squid.conf, copy
it to /etc/squid3/, and activate it:

``sudo make``

If that worked, TEST IT. Visit the URL in question, but also some other
URLs on other servers which are handled by this cache, e.g.
http://publicdata.eu/

If squid turns out to be broken, revert. This should bring it back to
the original situation:

| ``hg revert squid.conf``
| ``sudo make``

If all went well, commit and push:

| ``hg ci -u $USER -m '[squid] Example commit message'``
| ``hg push``

squid.conf
==========

Our squid.conf is straightforward. For each web server (called "peer")
it caches, there is a standard set of lines. Lets make an example,
s025/eu25 which runs publicdata.eu:

``$  grep s025 ~okfn/etc/squid3/squid.conf ``

| ``cache_peer s025-int.okserver.org parent 80 0 no-query originserver name=s025 login=PASS``
| ``s025/eu25``
| ``acl s025_sites dstdomain publicdata.eu``
| ``acl s025_sites dstdomain www.publicdata.eu``
| ``acl s025_sites dstdomain static.publicdata.eu``
| ``http_access allow s025_sites``
| ``cache_peer_access s025 allow s025_sites``
| ``cache_peer_access s025 deny all``

Remarks:

-  The lines per peer are a bit distributed to be grouped.
-  For peers at Amazon EC2, we use a CNAME s0xx-int.okserver.org ==>
   ec2-yyyyy.compute.amazonaws.com which resolves to either the peer's
   public or private IP address, depending on whether the cache lives in
   the same EC2 datacenter and could access the peers private address.
-  "login=PASS" means that the "Authenticate:" headers doesn't get
   stripped.

For more information in squid.conf, see

-  http://wiki.squid-cache.org/SquidFaq/ConfiguringSquid
-  `Sample squid.conf <http://pastebin.com/raw.php?i=sDEEiJJi>`__

Notes
=====

-  http://lists.okfn.org/pipermail/okfn-help/2010-September/000844.html
-  This article should contain a comparison "squid vs. varnish"

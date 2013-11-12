SSL for Apache Howto
====================

SSL certs
---------

-  To use SSL you actually need a certificate \*and\* a private key
   (otherwise how is communicate encrypted?).
-  things required to to create a valid certificate:

   -  A root certficiate and associated private key
   -  A new certficate signed by the private key of the root certificate

Generate Root Certificate
~~~~~~~~~~~~~~~~~~~~~~~~~

-  Pick a directory where you want your root CA stuff to be stored.
-  The default in *openssl.cnf* is *./demoCA* and we shall proceed
   assuming that is being used, this generates a certificate valid for
   10 years.

``openssl req -new -x509 -keyout root.key -out root.crt -days 3650 ``

-  Note this will request a password for your private key. This should
   be reasonably secure as the security of your root certificate (and
   everything signed by that) depend on it.
-  Now ensure that the file *index.txt* is empty and that the file
   serial contains 01 (both in *./demoCA*).

Generate Signed Certificate
---------------------------

-  Generate a new key and unsigned certificate:

| ``openssl req -new -keyout new.key -out newreq.pem -days ``\ \ `` ``
| *``NB:`` ``the`` ``password`` ``for`` ``the`` ``private`` ``key``
``need`` ``not`` ``be`` ``very`` ``secure`` ``here`` ``as`` ``it``
``only`` ``relates`` ``to`` ``this`` ``individual``
``certificate``*\ ``  ``
| ``''NB: difference between this and root certificate is no -x509 option  ``

-  Now sign unsigned certificate with the root cert made above''

``openssl ca -policy policy_anything -out newcert.pem -cert root.crt -keyfile root.key -in newreq.pem  ``

-  For this to work you may to reconfigure */etc/ssl/openssl.cnf* as it
   is likely to be broken (e.g. pointing to *./demoCA* directory that
   does not exist).

   -  You probably want the policy **policy\_anything**, which imposes
      no conditions on the relation of the certificate request to the
      root certificate (see *openssl.cnf* for more details).

-  [optional] you may want to remove the passphrase from the private
   key:

``openssl rsa -in new.key -out new.unsecure.key  ``

-  Make sure you now restrict access to this file by changing the file
   permissions (for example make it only readable by Apache user).

Simpler Method
--------------

-  Edit *openssl.cnf* to set your certificate authority directory (by
   default it is *./demoCA*).

-  Generate root stuff and distribute it in accordance with the layout
   in *openssl.cnf*.
-  Then from now on you do not need to specify root ca stuff, the rest
   is as above

HowTo Setup Apache and SSL On Debian (OLD)
------------------------------------------

-  Consists of two steps

   -  Generating SSL certificates
   -  setting up apache to use this
   -  reference certificate in VirtualHost directive in config file
   -  Enable port 443 (https port) - edit ports.conf

Generating SSL Certificates
---------------------------

-  Follow [1].
-  If you use /*CA.pl* output is in /*demoCA*

-  You need to create:

   -  your own self-signed root certificate authority certificate
   -  certificate: *cacert.pem*
   -  key: *private/newreq.pem*

-  For each certificate:

   -  create a request (out: *newreq.pem*) for
   -  you then sign with your root certificate producing: newcert.pem
   -  so you now have key (newreq.pem) and certificate (newcert.pem)

-  Optional:

   -  strip certificate request section from newreq.pem (no longer
      needed) to leave just the key.

-  You may also want to strip the passphrase from the private key:
   *openssl rsa -in newreq.pem -out wwwkeyunsecure.pem* (you must then
   change permissions to restrict access)

-  suggest then renaming everything as follows

   -  .key (newreq.pem), .insecure.key (wwwkeyunsecure.pem)

-  .cert (newcert.pem) (also you can create stripped down version with
   just actual certificate rather than the metadata as .crt

References
~~~~~~~~~~

-  http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/
-  http://forums.devshed.com/archive/t-22189 - some info on difference
   between mod\_ssl and Apache-SSL 1.
-  http://myrddin.org/howto/debian-apache-sslcert.html - short and to
   the point but not as clear as it might be.
-  http://www.webhostingtalk.com/showthread.php?threadid=241850 -
   another how-to. a little short
-  http://httpd.apache.org/docs-2.0/ssl/ssl_faq.html#aboutcerts

Certificate Authentication
--------------------------

-  Apache offers a variety of authentication methods:

   -  BasicAuth -

      -  This the simplest and over SSL this is reasonably secure.
         However does require user to always enter their details (i
         think) which is annoying if you are using a program over https
         (such as svn)

   -  Certificate authentication. See [1] and [2]. In [1] see
      SSLVerifyClient Directive and SSLVerifyDepth Directive

Trusted Certificates
--------------------

-  Every SSL certificate used by every website is derived from a 'root
   certificate'.
-  Certificates derived from 'trusted' root certificates cost money.
-  Default generated certificates do not derive from a trusted root,
   this means that any user who browses to https://... will receive a
   message saying 'this certificate is not trusted, continue? yes/no'.
-  This doesn't matter for internal purposes, but if you want to set up
   public https access, or if you want to take credit card details via
   https, one needs to buy a certificate. http://www.instantssl.com/
   offer the cheapest - about $50/year.

references
~~~~~~~~~~

-  `mod\_ssl <http://httpd.apache.org/docs-2.0/mod/mod_ssl.html>`__
-  `SSL howto <http://httpd.apache.org/docs-2.0/ssl/ssl_howto.html>`__

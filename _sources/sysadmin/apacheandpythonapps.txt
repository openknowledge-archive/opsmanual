How to Deploy Python Apps (esp. Pylons) Using Apache
====================================================

Proxy Method
------------

-  Enable/install relevant modules

| ``a2enmod proxy``
| ``a2enmod proxy_http``
| ``a2enmod proxy_html``

-  Now put config like this in your apache vhost section (assuming your
   running on localhost:5000):

| ``# Proxy approach``
|
| ``# . should be there!``
| ``Allow from .{your-domain}``
|
| ``ProxyPass / ``\ ```http://localhost:5000/`` <http://localhost:5000/>`__
| ``ProxyPassReverse / ``\ ```http://localhost:5000/`` <http://localhost:5000/>`__
| ``SetOutputFilter  proxy-html``
| ``ProxyHTMLURLMap ``\ ```http://localhost:5000`` <http://localhost:5000>`__\ `` ``\ ```http://{your-domain}`` <http://{your-domain}>`__

Modpython
---------

-  See
   http://wiki.pylonshq.com/display/pylonscookbook/Production+deployment+using+mod_python

| ``# mod_python method``
| ``# path = path to your application directory``
| ``# path-config = path to your paste/pylons config file``
| ``DocumentRoot {path}``
| ``SetHandler python-program``
| ``PythonHandler paste.modpython``
| ``PythonOption paste.ini {path-config}``

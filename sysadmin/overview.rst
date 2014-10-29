Infrastructure overview
=======================

Open Knowledge operates many different services for many different people. This
document is intended to provide a broad overview of the infrastructure
underlying these services and how it is managed.

The infrastructure operated by Open Knowledge (and specifically that managed by
the Sysadmin team) can broadly be divided into two categories:

#. `Self-hosted infrastructure`_
#. `Externally-hosted infrastructure`_

This document describes these two areas below:

Self-hosted infrastructure
--------------------------

Machine hosting
~~~~~~~~~~~~~~~

We maintain a reasonable number of (primarily Ubuntu Linux) servers across a
number of hosting providers. Without exception, these machines should be managed
by our Ansible configuration management system.

Please refer to the `Ansible inventory`_ for the canonical list of managed
hosts and their whereabouts.

.. _Ansible inventory: https://github.com/okfn/infra/blob/master/inventory/hosts

Mailing lists
~~~~~~~~~~~~~

We maintain a Mailman_ instance which manages a wide variety of public and
private mailing lists on the ``lists.okfn.org`` mail domain.

.. _Mailman: http://www.list.org/

Etherpad
~~~~~~~~

Etherpad_ is an online document editor providing collaborative real-time
authoring. We host an instance at http://pad.okfn.org.

.. _Etherpad: http://etherpad.org/

Wiki
~~~~

There is a somewhat under-loved MediaWiki instance available at
http://wiki.okfn.org


Externally-hosted infrastructure
--------------------------------

Mail routing
~~~~~~~~~~~~

We use FastMail_ as our front-line mail router for almost all mail domains
associated with OKF (``okfn.org``, ``ckan.org``, etc.). One notable exception is
the mailing list server (``lists.okfn.org``) described above: `Mailing lists`_

.. _FastMail: https://www.fastmail.fm

Staff email
~~~~~~~~~~~

Open Knowledge Central staff email, calendars, etc. (often collectively known
as "groupware") is currently provided by `Google Apps for Business`_.

.. _Google Apps for Business: http://www.google.com/enterprise/apps/business/

We also now have Google Vault_.  Which can be used to 'save' emails from email accounts which aren't in use.  

.. _Google Vault: https://support.google.com/vault/answer/2462365?hl=en&ref_topic=2739742

WordPress "farm"
~~~~~~~~~~~~~~~~

A multi-domain WordPress_ "farm" is operated for us by WPEngine_. This hosts,
among others, ``blog.okfn.org``, as well as many other project and
local group sites. Please note that this **does not** host the `okfn.org`
website.

.. _WordPress: http://wordpress.org
.. _WPEngine: http://wpengine.com

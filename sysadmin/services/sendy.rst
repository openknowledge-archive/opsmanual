Sendy: announce list and newsletter service
###########################################

For managing large or frequent mailouts, we run a self-hosted version of Sendy_
at https://sendy.okfn.org. Sendy (which sends email through `Amazon SES`_) is
dramatically cheaper than equivalent commercial offerings. ($0.10 per 1000
mails, vs. ~$8-30 per 1000 mails for Mailchimp).

.. _Sendy: http//sendy.co

Details
=======

Our Sendy instance is hosted on Heroku (under the main sysadmin account account:
``opensendy``), and the customized source code is kept in a private GitHub
repository: https://github.com/okfn/opensendy.

It is important to note that Sendy is **not** open-source software, and thus
this source code repository must remain private.

Overview
========

Sendy has three main parts:

*Campaign*
  Campaigns are specific mailings (i.e. specific emails), which are sent to one
  or more...

*List*
  Each *List* is a set of recipient email addresses, and associated metadata on
  previous bounces, complaints, etc.

Both *Campaigns* and *Lists* are associated with exactly one

*Brand*
  A *Brand* is the top-level identity of the mailout (e.g. "Widgets Monthly")
  and are tied to a username and password which allow for management of all
  aspects of the *Brand*.

Sendy also has an administrator account, the details of which are kept in the
``team-sysadmin`` shared folder on LastPass.

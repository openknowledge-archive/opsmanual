Mailman on Debian
=================

Renaming Lists
--------------

See this FAQ (or just search!):
http://wiki.list.org/pages/viewpage.action?pageId=4030617

Mailman
-------

-  Edit mm\_cfg.py (in /etc/mailman/)
-  In particular make sure url setup here coincides with that in apache.
   e.g. I set IMAGE\_LOGOS = '/mmimages/' to coincide with what I set in
   apache config (see below).

Exim4 Howto
-----------

-  apt-get mailman -t unstable for mailman 2.1 (this is 20041001 and
   using exim4 so want unstable)
-  Start with this which is exim4's own docs:
   http://www.exim.org/howto/mailman21.html
-  This is **excellent**
   http://mail.python.org/pipermail/mailman-users/2004-June/037419.html,
   Suggests naming for files in debian's new exim4 file layout.
-  For macros part of exim4 i tried MM\_UID=mail and MM\_GID=list and
   this seemed to work.
-  **IMPORTANT** With default setup mailman would send out admin mails
   (e.g. about subscriptions) but would freeze all mails sent to list.
   This was fixed by modifying the following exim4 file
   conf.d/acl/30\_exim4-config\_check\_rcpt as follows (see [2.1]):

| ``acl_check_rcpt:   ``
| ``#Accept if the source is local SMTP (i.e. not over TCP/IP). ``
| ``#We do this by testing for an empty sending host field.   ``
| ``accept hosts = :    ``
| ``#INSERTED BY RP 2004-10-09 to deal with mailman   ``
| ``accept  hosts = 127.0.0.1``

Setting up Apache
-----------------

-  Follow [1] but replace /usr/lib/mailman throughout by
   /var/lib/mailman

Test it
-------

-  Follow [1]

Multiple Domains on One Machine
-------------------------------

-  If you can ensure having list names that don't conflict across
   domains this is very simple. Just add another add\_virtual\_host item
   in /etc/mailman/mm\_cfg.py
-  Remember to add list through web interface rather than from server
   admin so that list is created in correct domain
-  Still doesn't give you seperate admin password for creating and
   deleting lists
-  o/w more complicated and probably involves installing one mailman
   instance for each domain (not sure about exim4 config). See [2]
-  For summary of what is and is not possible in mailman 2.
-  see [3]

Bulk operations on mailing lists.
=================================

Nick asked if we could normalise all the names of the mailing lists to
lowercase. It turns out to be pretty easy. Create a python file like
this, and call it `lowername.py`::

  def lowername(mlist):
    mlist.real_name = mlist.real_name.lower()
    mlist.Save()

Then this can be run with the `withlist` program::

  export PYTHONPATH=`pwd`
  ls /var/lib/mailman/lists | while read list; do
      withlist -l -r lowername $list
  done

Biblio
======

`Mailman with Exim on Woody
Howto <http://www.linuxgazette.com/issue91/price.html>`__
`1 <http://www.us.exim.org/howto/mailman21.html>`__
`2 <http://www.us.exim.org/howto/mailman21.html#retune>`__
`3 <http://mail.python.org/pipermail/mailman-users/2004-March/035277.html>`__

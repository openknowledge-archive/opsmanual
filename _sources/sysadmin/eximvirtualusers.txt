Intro
=====

This describes setting up a (debian) email system to deliver email for
virtual users (i.e. for users with email accounts but no shell account).
It explains both how to deliver mail and how to set up imap and pop to
allow users to pick it up.

Software:

-  Exim MTA
-  Courier (Imap and Pop)
-  [Optional] a webmail program

Requirements:

-  Wish to provide email routing and access
-  Want to be able to run off centralized information set

Two steps in this process:

-  Routing email as it arrives
-  Setting up courier pop and imap to pick it up.

These steps are independent in theory but we will want to have only a
single set of config files driving both processes. The method of choice
will be built around courier userdb files which is a relatively
'low-tech' solution and avoids the use of a direct connection to a db
(which can be awkward as such support must be compiled into exim and
courier).

NB: many of these options benefit from having some kind of alias
director (e.g. for catch all routing). An alias director allows
rerouting of domains to shell users e.g. \*@xyz.com: joe. Two docs on
how to do this are (implementing a very similar solution)

-  http://www.wlug.org.nz/EximNotes section called: Configuring Exim4
   with a virtual domain table/users in text files

Routing Incoming Email
======================

Two choices about how you do this

#. use exim to do the virtual domain and user routing. See [1]
#. route it all to a real user and then use a .forward file in that
   directory

Option 1 is preferable though 2 is included as it could be easier in a
very simple case of only 1 domain on a system and also provides useful
examples of .forward files.

Implementing Option 1
---------------------

-  In exim.conf put this in your Director section near the top
   (certainly before localuser and any aliasing director you may have):

| ``virtualuser_delivery:``
| ``driver = appendfile``
| ``# this looks up the correct shell user account to use when writing mail``
| ``user = ${lookup {$domain} lsearch {/etc/exim/virtual/domains} {$value}}``
| ``# the default exim mail user``
| ``group = mail``
| ``# write files and directories with permissions 770``
| ``directory_mode = 0770``
| ``mode = 0770``
| ``mode_fail_narrower = false``
| ``maildir_format = yes``
| ``# we write to /home/$user_for_which_domain_runs/Maildir/$local_part``
| ``# if we were using a generic system we might use /var/virtual_mail/ instead``
| ``directory = /home/${lookup {$domain} lsearch {/etc/exim/virtual/domains} {$value}}/Maildir/$local_part``
| ``create_directory = yes``
| ``delivery_date_add = yes``
| ``envelope_to_add = yes``
| ``return_path_add = yes``

-  In the transports section (remember order does not matter)

| ``virtualuser:``
| ``driver = smartuser``
| ``# check this is in the virtual domain list``
| ``domains = lsearch;/etc/exim/virtual/domains``
| ``# check that this local user exists``
| ``local_parts = lsearch;/etc/exim/virtual/$domain``
| ``transport = virtualuser_delivery``

Organization:

| ``userdb files put in directory /etc/courier/userdb``
| ``exim config info: /etc/exim/virtual directory``

Steps:

-  Create userdb file for the domain (see
   http://www.inter7.com/courierimap/INSTALL.html#userdb).
-  Put in /etc/courier/userdb and name the file ${domain} e.g. xyz.com
-  Run extract script on it (see below for script), this will take info
   from userdb files and dump it into right place for exim (currently
   /etc/exim/virtual)
-  create entry in file /etc/exim/virtual/domains of the form $domain:
   [$corresponding\_user] ([ ... ] indicates optional)
-  If not done automatcially then copy file generated in 2 to
   /etc/exim/virtual
-  chown -R $user:mail /home/$user/Maildir and make sure permissions are
   660 or 770.
-  This is to ensure the exim can deliver mail okay and the user can
   read it
-  OPTIONAL: test exim by running exim -d2 -bt and then entering test
   email addresses. Should pick up virtual users.

Script to extract exim delivery tables from courier userdb

| ``#!/bin/bash``
| ``# exim deliverytables from the courier userdb``
| ``COURIERDIR="/etc/courier"``
| ``USERDBDIR="$COURIERDIR/userdb"``
| ``EXIMDIR="/etc/exim"``
| ``DELITABLEDIR="$EXIMDIR/virtual"``
| ``mkdir -p $DELITABLEDIR``
| ``cd $USERDBDIR``
| ``for domain in *; do``
| ``  FILE="$DELITABLEDIR/$domain"``
| ``    echo  > $FILE "# exim delivery table"``
| ``    echo >> $FILE "# generated by $0" >> $FILE``
| ``    echo >> $FILE -n "# from $USERDBDIR/$domain on "``
| ``    date >> $FILE +"%Y-%m-%d %k:%M:%S"``
| ``    echo >> $FILE "# do not edit"``
| ``    echo >> $FILE``
| ``   < $domain sed 's/``\ :math:`[^@]*`\ ``.*/\1/' >> $FILE``
| ``done``

Old Version of Option 1
~~~~~~~~~~~~~~~~~~~~~~~

-  Follow `Virtual Users with Exim and Courier
   IMAP <http://www.alexlomas.com/info/exim-courier-virtualusers.html>`__

| ``#This router matches virtual_users mailboxes``
| ``virtual_user:``
| ``driver = accept``
| ``domains = +local_domains``
| ``local_parts = lsearch:/etc/virtual_users_$domain``
| ``transport = virtual_localuserdelivery``

-  Add this anywhere in your transports section (order is irrelevant):
-  This transport is for virtual user delivery after checking for local
   delivery

| ``virtual_localuserdelivery:``
| ``driver = appendfile``
| ``user = courier``
| ``maildir_format``
| ``directory = /home/courier/$domain/$local_part``
| ``create_directory``
| ``delivery_date_add``
| ``envelope_to_add``
| ``return_path_add``
| ``group = mail``
| ``mode = 0660``

-  Note that that router means director in the above.
-  This is fine. My only problem was

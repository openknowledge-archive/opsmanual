Mailing list hosting
####################

-  Runs on `GNU Mailman <http://www.gnu.org/s/mailman/index.html>`__
-  Deployed at: http://lists.okfn.org/
-  Details of setup/configuration at: `MailmanHowto <MailmanHowto>`__
-  List creation password: std low-sec pwd

How to add a new mailing list domain
====================================

-  **NOTE**: While its possible to create multiple domains in mailman,
   its upto the admin/users to ensure lists with duplicate names are not
   created.

-  The current default is @lists.okfn.org
-  To add more domains, append the domain name into the below set of
   files

| ``/etc/exim4/conf.d/main/01_exim4-config_listmacrosdefs``
| ``domainlist local_domains = @:localhost:lists.okfn.org:lists.knowledgeforge.net:lists.dataexpeditions.org:partial-lsearch;/etc/mailconfig/mail_domains``

| ``/conf.d/main/04_exim4-config_mailman``
| ``domainlist mm_domains=lists.okfn.org:lists.knowledgeforge.net:lists.dataexpeditions.org``

-  Ensure that the new domain has been added into the MTA as mentioned
   `here <http://wiki.okfn.org/Sysadmin/EmailService#How_to_add_a_new_email_domain>`__
-  If the domain was just added into exim's config files, you may need
   to reload exim.

How to Rebuild list archives
============================

``mmarch ``\

How to Rename a List
====================

-  See mailman documentation :

   -  `How to Rename a
      List <http://How%20to%20Rename%20a%20List%20in%20http://www.gnu.org/software/mailman/faq.html>`__
   -  `How do I change the name of (rename) a
      list? <http://wiki.list.org/pages/viewpage.action?pageId=4030617>`__

-  *Note that some files and dirs need to be readable and/or writeable
   by user/group www-data, others by user/group "list".*

   -  See e.g. `trac-ticket <http://trac.okfn.org/ticket/1198>`__ where
      a rename left /var/lib/mailman/lists/$LIST/config.pck not being
      accessible by "list".

How to remove an individual post
================================

'' This is quite painful. See also'' `How can I remove a post from the
list archive or remove an entire
archive? <http://wiki.list.org/pages/viewpage.action?pageId=4030681>`__

-  Log into our mailman host (currently *s001.okserver.org*) and follow
   the below steps:

| ``# What list do we want to edit? ``
| `` ML="sysadmin-coord"  ``
| ``# Make a backup of the mbox file ``
| `` cp -a /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox{,.SAVE-`date +%Y%m%d`}  ``
| ``# Make a working copy of the mbox file ``
| `` cp -a /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox{,.work}  ``
| ``# Enter the mbox and remove the posting. ``
| ``#BEWARE: *REMOVING* the post will cause renumbering of the subsequent posts in the archive.``
| ``#You might want to *EDIT* the post instead and remove its content. Not sure whether this can be done with mutt though.``
| `` mutt -f /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.work  ``
| ``# Remove the new headers mutt has left behind: ``
| ``#BEWARE 1: Maybe other statuses are also possible, e.g. "R". Make sure they are removed as well``
| ``#BEWARE 2: This would also remove such lines from mail BODIES which would be wrong!``
| `` egrep -v '^(Status: O|Content-Length: [0-9]*|Lines: [0-9]*)$' /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mboxwork > /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox ``
| ``#Check that we really only removed the one post: ``
| `` diff /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox{,.work}  ``
| ``#Is all fine? The proceed: ``
| ``#Remove the working copy ``
| `` rm /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.work  ``
| ``#move the old HTML archive: ``
| `` mv /var/lib/mailman/archives/private/${ML}{,.SAVE-`date +%Y%m%d`}  ``
| ``#re-create html archive:``
| `` sudo -u www-data mkdir -m 775 /var/lib/mailman/archives/private/${ML} sudo -u list /usr/lib/mailman/bin/arch ${ML}``

How to remove a user from a mailinglist via cli
===============================================

``remove_members -n -N ``\ \ `` ``\

How to add a user into a mailling list via cli
==============================================

``echo  '``\ \ ``' | add_members -r - -wn ``\

How to export mailinglist users
===============================

-  To list users of a particular list, with full names - *'Full Name'*

| ``list_members --fullnames --``\ \ `` ``\
| ``e.g # list_members --fullnames --regular okfn-announce``

-  To convert the list into csv separted format like - ',,'

``sed -e 's/^``\ ``[^<]*``\ ``$/<\1>/g' -e 's/ *</,/g' -e 's/>$//g' -e 's/\"//g' awk -F ',' "{print \$2 \",\" \$1 \",${type}\" } | sort``

How to upgrade mailman
======================

Mailman does not like to be upgraded while it has messages in its
queues. Therefore you should follow this procedure:

-  Stop mailman.

-  If you want, make a backup of the queues

``/var/lib/mailman/qfiles/{bad,shunt}/ ``

-  Remove "bad" and "shunt" queued messages:

``sudo /usr/lib/mailman/cron/cull_bad_shunt ``

-  Check whether there are still queue files:

``sudo find /var/lib/mailman/qfiles/ -type f ``

If there are no more queued messages, you can upgrade mailman now.
Otherwise proceed:

-  Prevent the MTA (exim in our case) from passing new postings to
   mailman, but make sure it still accepts mails \*from\* mailman. I am
   not sure whether stopping the exim service would work, so instead i
   block port 25 temporarily:

``sudo iptables -A INPUT -m state --state NEW -p tcp --dport 25 -i ! lo -j REJECT ``

-  Start mailman and wait until the queues are empty:

``sudo watch 'find /var/lib/mailman/qfiles/ -type f | wc -l'``

-  Stop mailman. Revert the above step that stopped exim from passing
   messages to mailman, e.g. start exim, or remove any block:

``sudo iptables -D INPUT -m state --state NEW -p tcp --dport 25 -i ! lo -j REJECT ``

Now it should be safe to upgrade mailman.

-- from http://trac.okfn.org/ticket/611

Mailman Troubleshooting
=======================

-  Important folders

mailman folder

``/var/lib/mailman/``

mailman private archives (all lists, mbox files)

``/var/lib/mailman/archives/``

mailman public archives (lists available via the web interface, html
files)

``/var/lib/mailman/archives/public``

individual list config (stored in python pickle format)

``/var/lib/mailman/lists/${list-name}/config.pck``

-  Reading list config

``/var/lib/mailman/bin/dumpdb /var/lib/mailman/lists/${list-name}/config.pck``

-  modifying a list config

| ``* Create a config file with content like 'key=value' pairs, key and value pairs can be read from the .pck file.``
| ``* In the config given below, we're modifying the list footer to include an unsubscribe link.``

| ``#cat /root/mailman_list_config``
| ``mlist.personalize=1  mlist.msg_footer='_______________________________________________\n%(real_name)s mailing list\n%(real_name)s@%(host_name)s\n%(web_page_url)slistinfo%(cgiext)s/%   (_internal_name)s\nUnsubscribe: %(web_page_url)soptions/%(_internal_name)s\n' ``
| ``mlist.digest_footer='_______________________________________________\n%(real_name)s mailing list\n%(real_name)s@%(host_name)s\n%(web_page_url)slistinfo%(cgiext)s/%(_internal_name)s\nUnsubscribe: %(web_page_url)soptionss/%(_internal_name)s\n' ``

| `` ##Apply the config to the list``
| ``/usr/sbin/config_list -i mailman_list_config ${list-name}``

Cleaning the Postfix Queue on the Mailman Server
================================================

Occasionally, postfix on the mailman server will have a large queue because of
rejections. Usually, there might a spam user who was sent emails which were
rejected several times ending up in the queue.

When this happens, run the following command to get a list of users and the
number of emails in the queue for them:

    postqueue -p | grep '@' | grep -v bounces | sort | uniq -c

Get the most offending user from this list and delete them from the queue with
the `postfix_queue_del.pl` script

    postfix_queue_del.pl spammer@example.com

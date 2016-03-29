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
-  To add more domains, append the domain name into ``ansible/inventory/host_vars/s116.okserver.org.yml``
   in the relevant parts of the file


How to Rebuild list archives
============================

``mmarch``

How to Rename a List
====================

See mailman documentation :

  - `How to rename a list <http://How%20to%20Rename%20a%20List%20in%20http://www.gnu.org/software/mailman/faq.html>`_
  - `How do I change the name of (rename) a list? <http://wiki.list.org/pages/viewpage.action?pageId=4030617>`_

**NOTE**: Some files and dirs need to be readable and/or writeable by
user/group www-data, others by user/group "list".

How to remove an individual post
================================

We do not remove emails from Open Knowledge list archives as a matter of
course.  As the email has already been sent to the list, so to all subscribers,
it is already public.  Also constantly removing emails places a lot of extra
work on the team.

It is possible to remove the email from the list archives, however this breaks
all links in the list archive.  Sysadmins will remove messages only for serious
privacy reasons; the current policy is that all requests are rejected (unless
there is a court order or equivalent legal obligation, of course).

This is quite painful. See also: `How can I remove a post from the list archive or remove an entire archive? <http://wiki.list.org/pages/viewpage.action?pageId=4030681>`_

Log into our mailman host (currently *s116.okserver.org*) and follow the below
steps::

    # What list do we want to edit?
    ML="sysadmin-coord"

    # Make a backup of the mbox file
    cp -a /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.`SAVE-date +%Y%m%d`

    # Make a working copy of the mbox file
    cp -a /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.work

    # Enter the mbox and remove the posting.
    #BEWARE: *REMOVING* the post will cause renumbering of the subsequent posts in the archive.
    #You might want to *EDIT* the post instead and remove its content. Not sure whether this can be done with mutt though.

    mutt -f /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.work
    # Remove the new headers mutt has left behind:
    # BEWARE 1: Maybe other statuses are also possible, e.g. "R". Make sure they are removed as well
    # BEWARE 2: This would also remove such lines from mail BODIES which would be wrong!

    egrep -v '^(Status: O|Content-Length: [0-9]*|Lines: [0-9]*)$' /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mboxwork > /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox

    # Check that we really only removed the one post:
    diff /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.{work}

    #Is all fine? The proceed:
    #Remove the working copy
    rm /var/lib/mailman/archives/private/${ML}.mbox/${ML}.mbox.work

    #move the old HTML archive:
    mv /var/lib/mailman/archives/private/${ML}{,.SAVE-date +%Y%m%d}

    #re-create html archive:
    sudo -u www-data mkdir -m 775 /var/lib/mailman/archives/private/${ML} sudo -u list /usr/lib/mailman/bin/arch ${ML}

How to remove a user from a mailinglist via cli
===============================================

``remove_members -n -N [addr1...]``

How to add a user into a mailling list via cli
==============================================

``echo  'person@example.com' | add_members -r -wn listname``

How to export mailinglist users
===============================

To list users of a particular list, with full names - *'Full Name'*::

    list_members --fullnames listname

How to upgrade mailman
======================

Mailman does not like to be upgraded while it has messages in its
queues. Therefore you should follow this procedure:

-  Stop mailman.

-  If you want, make a backup of the queues::

    /var/lib/mailman/qfiles/{bad,shunt}/

-  Remove "bad" and "shunt" queued messages::

    sudo /usr/lib/mailman/cron/cull_bad_shunt

-  Check whether there are still queue files::

    sudo find /var/lib/mailman/qfiles/ -type f

If there are no more queued messages, you can upgrade mailman now.
Otherwise proceed:

-  Prevent the MTA (postfix in our case) from passing new postings to
   mailman, but make sure it still accepts mails \*from\* mailman. I am
   not sure whether stopping the postfix service would work, so instead i
   block port 25 temporarily::

    sudo iptables -A INPUT -m state --state NEW -p tcp --dport 25 -i ! lo -j REJECT

-  Start mailman and wait until the queues are empty::

    sudo watch 'find /var/lib/mailman/qfiles/ -type f | wc -l'

-  Stop mailman. Revert the above step that stopped postfix from passing
   messages to mailman, e.g. start postfix, or remove any block::

    sudo iptables -D INPUT -m state --state NEW -p tcp --dport 25 -i ! lo -j REJECT

Now it should be safe to upgrade mailman.

Mailman Troubleshooting
=======================

Important folders

- Mailman folder::

    /var/lib/mailman/

- Mailman private archives (all lists, mbox files)::

    /var/lib/mailman/archives/

- Mailman public archives (lists available via the web interface, html files)::

    /var/lib/mailman/archives/public

- Individual list config (stored in python pickle format)::

    /var/lib/mailman/lists/${list-name}/config.pck

Reading list config::

    /var/lib/mailman/bin/dumpdb /var/lib/mailman/lists/${list-name}/config.pck

Modifying a list config

- Create a config file with content like 'key=value' pairs, key and value pairs
  can be read from the .pck file.
- In the config given below, we're modifying the list footer to include an
  unsubscribe link.

::

    cat /root/mailman_list_config
     mlist.personalize=1  mlist.msg_footer='_______________________________________________\n%(real_name)s mailing list\n%(real_name)s@%(host_name)s\n%(web_page_url)slistinfo%(cgiext)s/%   (_internal_name)s\nUnsubscribe: %(web_page_url)soptions/%(_internal_name)s\n'
     mlist.digest_footer='_______________________________________________\n%(real_name)s mailing list\n%(real_name)s@%(host_name)s\n%(web_page_url)slistinfo%(cgiext)s/%(_internal_name)s\nUnsubscribe: %(web_page_url)soptionss/%(_internal_name)s\n'
      # Apply the config to the list
     /usr/sbin/config_list -i mailman_list_config ${list-name}

Cleaning the Postfix Queue on the Mailman Server
================================================

Occasionally, postfix on the mailman server will have a large queue because of
rejections. Usually, there might a spam user who was sent emails which were
rejected several times ending up in the queue.

When this happens, run the following command to get a list of users and the
number of emails in the queue for them::

    postqueue -p | grep '@' | grep -v bounces | sort | uniq -c

Get the most offending user from this list and delete them from the queue with
the `postfix_queue_del.pl` script::

    postfix_queue_del.pl spammer@example.com


Renaming lists
==============

See this FAQ: http://wiki.list.org/pages/viewpage.action?pageId=4030617


Regenerate list archive HTML
============================

In some cases, the [HTML archive](https://lists.okfn.org/pipermail/<list-name>/) because a post had to be removed from the list or,
Mailman doesn't stops updating the HTML archive,

To regenerate the HTML archive ::

  /usr/lib/mailman/bin/arch --wipe <list-name>

  --wipe clears out the old HTML content


Bulk operations on mailing lists
================================

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


Common list moderator password
==============================

We needed a better way to manage moderator passwords across lists, since
mailman keeps one password - for multiple moderators on a list and this results
in passwords being required to be reset and additional overhead.

The solution we came up with, was to have a shared password for all list
moderators, and have it reset once every six months.

The script used to reset the moderator password across all lists is [/usr/lib/mailman/bin/change_mod_pw](https://github.com/okfn/infra/blob/master/ansible/roles/mailman/files/change_mod_pw)

- To update the moderator password for all lists::

    /usr/lib/mailman/bin/change_mod_pw -a -p <newpasswd>

- To update the moderator password for a single list::

   /usr/lib/mailman/bin/change_pw -l <listname> -p <password>


Run with --help for other options, you may require -q if the moderators do not need to be informed.

Archive a Mailing List
======================

- Disable delivery to the mailing list using the aliases file. This is done by
  taking the mailing list address to `/dev/null` in `src/infra/ansible/roles/postfix/templates/s116.okserver.org/aliases` and then running ansible.
  it is a good idea to also redirect the meta-addresses, `-owner` and `-request`
  to `/dev/null` as well.
- Disable public advertisement of the list either by using the list administrative
  interface or by doing::

    /usr/lib/mailman/bin/config_list -o /tmp/foo.py foo-list
    sed -i 's@^advertised = 1$@advertised = 0@' /tmp/foo.py
    sed -i 's@^emergency = 0$@emergency = 1@' /tmp/foo.py
    /usr/lib/mailman/bin/config_list -i /tmp/foo.py foo-list

  the second sed command is a belt and braces approach to disabling posting to the
  list, in addition to the aliases trick.
- Unsubscribe all members from the list::

    /usr/lib/mailman/bin/list_members foo-list | xargs /usr/lib/mailman/bin/remove_members foo-list


Import a mailinglist into the OKF mailman server
================================================

In some cases, a mailman list needs to be imported into the OKF mailman server,

- To import a single list, from a remote host we require::

  /var/lib/mailman/archives/private/<listname>.mbox
  /var/lib/mailman/lists/<listname>/config.pck

- Once the files are copied over to the OKF mailman server

  - We create a new list, the admin user doesn't matter as of now::

    /usr/sbin/newlist

  - Copy the mbox file::

      cp /tmp/<listname>.mbox /var/lib/mailman/archives/private/<listname>.mbox/<listname>.mbox

  - Generate HTML archives::

    /var/lib/mailman/bin/arch --wipe <listname>

  - Copy over the config.pck file which contains the users info and list config::

      cp /tmp/config.pck /var/lib/mailman/lists/<listname>/config.pck

  - Update the list url in config.pck::

    /var/lib/mailman/bin/withlist -l -r fix_url <listname> https://lists.okfn.org/mailman/

-  The admin interface, html archives should now be accessible with all its users::

    https://lists.okfn.org/mailman/admin/<listname>
    https://lists.okfn.org/pipermail/<listname>

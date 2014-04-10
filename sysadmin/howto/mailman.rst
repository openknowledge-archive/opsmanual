HOWTO: Administering Mailman
============================

Renaming lists
--------------

See this FAQ: http://wiki.list.org/pages/viewpage.action?pageId=4030617


Regenerate list archive HTML 
----------------------------

In some cases, the [HTML archive](https://lists.okfn.org/pipermail/<list-name>/) because a post had to be removed from the list or,
Mailman doesn't stops updating the HTML archive, 

To regenerate the HTML archive ::

  /usr/lib/mailman/bin/arch --wipe <list-name>

  --wipe clears out the old HTML content


Bulk operations on mailing lists
--------------------------------

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
------------------------------

We needed a better way to manage moderator passwords across lists, since mailman keeps one password - 
for multiple moderators on a list and this results in passwords being required to be reset and additional overhead.

The solution we came up with, was to have a shared password for all list moderators, and have it reset once every six months.

The script used to reset the moderator password across all lists is [/usr/lib/mailman/bin/change_mod_pw](https://github.com/okfn/infra/blob/master/ansible/roles/mailman/files/change_mod_pw)

- To update the moderator password for all lists::

    /usr/lib/mailman/bin/change_mod_pw -a -p <newpasswd>

- To update the moderator password for a single list::

   /usr/lib/mailman/bin/change_pw -l <listname> -p <password> 


Run with --help for other options, you may require -q if the moderators do not need to be informed.



Archive a Mailing List
----------------------

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
------------------------------------------------

In some cases, a mailman list needs to be imported into the OKF mailman server,

- To import a single list, from a remote host we require::
  
  /var/lib/mailman/archives/private/<listname>.mbox
  /var/lib/mailman/lists/<listname>/config.pck 

- Once the files are copied over to the OKF mailman server

  - We create a new list, the admin user doesn't matter as of now::
     
      /usr/sbin/newlist

  - We import the mbox file::
     
     /var/lib/mailman/bin/arch <listname> /tmp/<listname>.mbox
    
  - Copy over the config.pck file which contains the users info and list config::

     cp /tmp/config.pck /var/lib/mailman/lists/<listname>/config.pck

  - Update the list url in config.pck::
     
     /var/lib/mailman/bin/withlist -l -r fix_url <listname> https://lists.okfn.org/mailman/

-  The admin interface, html archives should now be accessible with all its users::

   https://lists.okfn.org/mailman/admin/<listname>
   https://lists.okfn.org/pipermail/<listname>

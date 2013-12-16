HOWTO: Administering Mailman
============================

Renaming lists
--------------

See this FAQ: http://wiki.list.org/pages/viewpage.action?pageId=4030617

Multiple domains on one machine
-------------------------------

-  If you can ensure having list names that don't conflict across
   domains this is very simple. Just add another ``add_virtual_host`` item
   in ``/etc/mailman/mm_cfg.py``
-  Remember to add list through web interface rather than from server
   admin so that list is created in correct domain
-  Still doesn't give you seperate admin password for creating and
   deleting lists

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

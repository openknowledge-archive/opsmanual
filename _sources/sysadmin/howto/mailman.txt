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

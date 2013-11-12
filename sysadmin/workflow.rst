This page documents how the sysadmin workflow should work. It's useful
to both the sysadmin team and the wider community as it explains how
processes work.

**Sysadmin helpdesk**
---------------------

-  The team can be reached at *sysadmin*\ @ okfn.org.
-  When sending mails to the help-desk, reporters are expected to
   include as much as details as possible w.r.t the help required.
-  When a mail is received on *sysadmin*\ @, the sysadmins create a
   'trac' (might be deprecated soon) ticket and cc the reporter, they
   will then proceed to prioritize it in their (trello) list.
-  Note: the'' blog-coord''@, *sysadmin-coord*\ @ aliases are only for
   internal communication between sysadmin team members, issue reports
   must **not** posted there.
-  The team prioritizes tasks based on importance w.r.t other issues and
   how quickly the task can be completed.

**Common helpdesk requests**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Email alias tasks (add, remove, modify)
-  Domain registration, modification of dns records
-  Implement backup processes
-  Mailing lists management (create/remove/modify lists, backups)

Service and/or Server request workfow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See `Service Life Cycle <Sysadmin/Service Life Cycle>`__

**Service issue/downtime notifications**
----------------------------------------

-  In case of a event which affects a lot of users e.g okfn.org is down,
   announcements should be sent out over mail and other communication
   mediums, depending on the impact.
-  The following doc may be used to identify which lists and aliases
   should be informed about the issue.
-  When projects service/s are affected sysadmins are expected to
   contact the project leads and/or whoever else is directly
   responsible.
-  Refer this doc to figure contact points for each project -

**Request prioritization**
--------------------------

-  A sysadmins task could include multiple items from monitoring to
   resolving issues.
-  The tasks being worked are part either the larger set of goals -
   Sysadmin Roadmap or daily recurring issues.

**Sysadmin Roadmap**
~~~~~~~~~~~~~~~~~~~~

-  The Roadmap can be found on the `sysadmin
   roadmap <https://trello.com/board/roadmap-sept-2012/5050c25b31b23a412014cf9b>`__
-  The roadmap is a high level overview of goals and projects, these
   mainly involve improving current processes and would involve multiple
   smaller tasks.
-  Examples of tasks in a roadmap would be, implementing better
   monitoring or configuration management, packaging etc.

**Backups and Disaster Recovery (WIP)**
---------------------------------------

-  Current backup method http://wiki.okfn.org/Sysadmin/Backup
-  VM snapshot backups
-  Webapp backups
-  Static page backups

-  Recovering using snapshots
-  Recovering from backups

-  DR process - who will have access

   -  How do we do dry-runs (?)
   -  Will this be automated

Category:Sysadmin

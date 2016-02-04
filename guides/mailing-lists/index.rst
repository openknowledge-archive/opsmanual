Managing mailing lists
======================

Introduction
------------

This document contains a description of how we manage a large set of community
mailing lists, and how you can perform common operations required of a list
owner or moderator.


The basics
----------

Our list server is known as ``lists.okfn.org``, and can be visited on the web at
https://lists.okfn.org.

A mailing list called ``widgets-discuss`` will have an email address of the form
``widgets-discuss@lists.okfn.org``. List owners can always be contacted by
emailing ``widgets-discuss-owner@lists.okfn.org``.

You can link to the ``widgets-discuss`` email address directly at
https://lists.okfn.org/mailman/listinfo/widgets-discuss (please note that this
particular link will not work, as we don't -- yet -- have a widget discussion
list).

List administrators and moderators
----------------------------------

List administrators and moderators are responsible for the smooth operation of
the list. Their tasks may include:

-  Helping users who have difficulty subscribing to or unsubscribing from their
   lists.
-  Banning abusive users.
-  Filtering spam that's sent to the list.
-  Ensuring that the list description and settings are kept up-to-date.

In particular, list administrators (also known as list owners) can do all of
these things and more. List moderators have a single role: reviewing the
moderation queue of the mailing list and letting through legitimate emails while
rejecting spam.

Obtaining a mailing list
------------------------

If you need a mailing list for your project, you need to make a few decisions
about your list, and then you can contact the Open Knowledge Foundation sysadmin
administrators for help. You should decide:

#. What you want your list to be called. A list to discuss geographic data might
   be called ``geodata-discuss``, while a local group list for Spain might be
   called ``okfn-es``. See the list names already taken at
   https://lists.okfn.org for ideas.
#. Do you want your list to be public or private? Should anyone be able to
   subscribe, or do you want to approve new sign-ups on a case-by-case basis?
#. Do you want your list to be publicly advertised in the list of lists at
   https://lists.okfn.org?
#. Who will be your list owners? Ideally you should have at least two.
#. Do you want to designate additional list moderators?

When you have answers to all these questions, you should contact the system
administrators with your request. Details on how to do this can be found here
on the Ops Manual: :doc:`/getting-help`.

Administering or moderating a mailing list
------------------------------------------

Say your list is called ``widgets-discuss``. You can visit the list information
page at https://lists.okfn.org/mailman/listinfo/widgets-discuss. From there, you
can select either the "administrative interface" or the "moderation interface"
links at the bottom of the page.

If you are a list administrator or a list moderator, you should have been
emailed a password allowing you to administer or moderate the list. Please check
your email archive for this email (it will come from ``mailman@lists.okfn.org``
and have a subject like "Your new widgets-discuss list password" or "Your new
widgets-discuss moderator password").

If you still can't find the password and think you can have it, please contact
the system administrators: :doc:`/getting-help`.


Moderating mailing list spam mail
---------------------------------

If the list you are moderating recieves spam, you can forward these spam mails from
the moderation interface to our bayes based spam learning address - ``learn-spam@lists.okfn.org``

This will help our mailman spam filter to 'learn' and classify spam mails correctly.


Mailing list actions that require sysadmin help
------------------------------------------------

You may request help from sysadmins for these mailing list queries

- Create a new list @lists.okfn.org
- Create a list on a different @domain
- Delete a list
- Keep list of subscribers
- Keep list archive
- Rename a list (copy subscribers over to a new list and archive the old one)
- Mailing list archives in a tarball/zip
- Reset the Admin password
- Export list subscribers as csv

HOWTO: Administering RT (Request Tracker)
=========================================

Prevent RT outgoing emails to a specified user
----------------------------------------------

Some email addresses from which RT receives email are ``noreply@`` email
addresses. By default, the emails which RT sends these users will usually create
additional noise in the form of "Undelivered Mail" notifications.

The easiest way to prevent these is to remove the email address from the
autocreated user. As long as the user's username still matches the email address
from which tickets are created, RT will correctly associate tickets with that
user, but won't attempt to email them.

1.  Go to :menuselection:`Admin --> Users --> Select` in the global navigation.
    (If you do not see an :guilabel:`Admin` button, you are not an RT
    administrator and will not be able to perform this task. Please contact
    someone who is.)
2.  Find the user by putting their email address in the :guilabel:`Go to user`
    box and submitting the form.
3.  Remove the user's email address from the :guilabel:`Email` form field.
4.  Save your changes using the :guilabel:`Save Changes` button at the bottom
    right of the form.

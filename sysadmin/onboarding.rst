Onboarding staff and managing leavers
=====================================

* Managers should email HR with details. For new joinees, it will involve
  sending an email with their full name and start date. For leavers, mention
  the name, email and final date. Also specify if emails need to be forwarded
  after the employee has left.
* HR will open a ticket with sysadmin about the new joinee. Sysadmins will
  create accounts and log actions to the ticket and close it out when
  everything is ready.

Joiners
-------



+-------------------------------------------------------+----------+--------+
| Action                                                | Team     | Req?   |
+=======================================================+==========+========+
| Create Google Apps account                            | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Add to relevant Google Groups                         | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Collect all new starter info, inc copy of Passport    | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Add email alias to FastMail, and wait 15m for alias   | sysadmin | Yes    |
| become live                                           |          |        |
+-------------------------------------------------------+----------+--------+
| Add a LastPass account and add to relevant LastPass   | sysadmin | Yes    |
| group                                                 |          |        |
+-------------------------------------------------------+----------+--------+
| Send Slack invitation                                 | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Send a Toggl invitation                               | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Send a Xero invitation                                | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Send TurbineHQ invitation                             | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Create new employee for Payroll                       | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Create Omnislip login ID for Payroll User             | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Setup Google Drive with the right folders             | employee | Yes    |
+-------------------------------------------------------+----------+--------+
| Add contact information to Contact spreadsheet        | employee | Yes    |
+-------------------------------------------------------+----------+--------+
| Email Laura, Rufus, or Fiona for adding into Know     | sysadmin | Yes    |
| Your Company                                          |          |        |
+-------------------------------------------------------+----------+--------+
| Send introuction email to all-staff@                  | manager  | Yes    |
+-------------------------------------------------------+----------+--------+
| Add to relevant Mailman lists                         | manager  | No     |
+-------------------------------------------------------+----------+--------+
| Github owners team access                             | manager  | No     |
+-------------------------------------------------------+----------+--------+
| Add to RT                                             | sysadmin | No     |
+-------------------------------------------------------+----------+--------+
| Access to blogfarm                                    | sysadmin | No     |
|                                                       | or       |        |
|                                                       | manager  |        |
+-------------------------------------------------------+----------+--------+
| Add keys to infra repo and grant access to servers    | sysadmin | No     |
+-------------------------------------------------------+----------+--------+
| Access to Highrise                                    | HR       | No     |
+-------------------------------------------------------+----------+--------+
| Calliflower                                           | HR       | No     |
+-------------------------------------------------------+----------+--------+
| Add to Team page on website                           | employee | No     |
+-------------------------------------------------------+----------+--------+


Leavers
-------

+-------------------------------------------------------+----------+
| Action                                                | Team     |
+=======================================================+==========+
| Suspend Google Apps account and transfer docs         | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Toggl                                     | HR       |
+-------------------------------------------------------+----------+
| Remove from Xero                                      | HR       |
+-------------------------------------------------------+----------+
| Remove contact information from Contact spreadsheet   | HR       |
+-------------------------------------------------------+----------+
| Remove account from Highrise                          | HR       |
+-------------------------------------------------------+----------+
| Remove account from Basecamp                          | HR       |
+-------------------------------------------------------+----------+
| Calliflower                                           | HR       |
+-------------------------------------------------------+----------+
| Remove from TurbineHQ                                 | HR       |
+-------------------------------------------------------+----------+
| Remove from Team page on website                      | manager  |
+-------------------------------------------------------+----------+
| Remove from Slack                                     | sysadmin |
+-------------------------------------------------------+----------+
| Remove from LastPass                                  | sysadmin |
+-------------------------------------------------------+----------+
| Remove from any aliases on FastMail                   | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Github owners team                        | sysadmin |
+-------------------------------------------------------+----------+
| Remove access to all blogfarm sites. Do not delete    | sysadmin |
| user. Downgrade to contributor on all sites           |          |
+-------------------------------------------------------+----------+
| Remove keys from infra repo and access to servers     | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Mailman lists                             | sysadmin |
| (`remove_members --fromall`)                          |          |
+-------------------------------------------------------+----------+
| Remove from RT                                        | sysadmin |
+-------------------------------------------------------+----------+
| Email Laura, Rufus, or Fiona for removing from Know   | sysadmin |
| Your Company                                          |          |
+-------------------------------------------------------+----------+

Managing Leavers Emails
-----------------------

There are three main ways we can deal with emails once someone has left.  We default to option 1, which is to disable the account.  We normally just disable the account initially and after a couple of months, the account is removed totally.  Once the account is removed, the emails are gone forever.  If you want either of the two other options, please ask.

- Option 1: We just disable the account and then when it's removed the email will bounce
- Option 2: We can setup a auto-response.  Please send the sysadmin team the full text of the auto-response.
- Option 3: We can redirect all emails from the user to another OK Staffer.  Please inform the staffer that you've requested this for them.

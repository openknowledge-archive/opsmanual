S3 is a good place for static content. This document is about how to
create and use a AWS S3 bucket for static content.

Create S3 bucket and user
-------------------------

... using AWS web console
~~~~~~~~~~~~~~~~~~~~~~~~~

-  Log into AWS console as administrative user, e.g. as
   "sysadmin@okfn.org"
-  **Create a new bucket**

   -  Go to tab "`S3 <https://console.aws.amazon.com/s3/>`__\ " and
      create a new bucket, e.g. "docs.ckan.org"
   -  Right-click on the new bucket, "Properties"
   -  In the "Properties" pane, go to sub-tab "Permissions" and grant
      "View permissions" (and maybe "List") to "Everyone"
   -  In the "Properties" pane, go to sub-tab "Website", "enable" it
      with "Index document = index.html". Write down the "Endpoint",
      e.g. http://docs.ckan.org.s3-website-eu-west-1.amazonaws.com/

-  **Create a group and allow it to access the new bucket**

   -  Go to the console main tab
      "`IAM <https://console.aws.amazon.com/iam/>`__\ " ==> "Groups"
   -  Create a new group, e.g. "docs.ckan.org-rw"
   -  Click on the new group "docs.ckan.org-rw", and go to the
      group-pane's sub-tab "Permissions". Press "Attach Policy" ==>
      "Policy Generator" and enter and "apply" this policy:

      -  Effect = Allow
      -  AWS Service = Amazon S3
      -  Actions = All actions \*
      -  Amazon Resource Name (ARN) = arn:aws:s3:::docs.ckan.org,
         arn:aws:s3:::docs.ckan.org/\*

   -  **TODO**: Restrict the policy so the user can do file and
      directory operations only, but e.g. not delete the bucket or
      change access permissions

-  **Create the user**:

   -  Go to the console main tab "IAM" ==> "Users"
   -  "Create new user", enter the user e.g. "docs.ckan.org-sync-user"
      and leave "Generate an access key for each User" ticked. When you
      get the dialog box "Your 1 User has been created successfully",
      click either "Download credentials", or "Show User Security
      Credentials" and write down the "Access Key Id" and the "Secret
      Access Key" for the new user.
   -  Add the new user "docs.ckan.org-sync-user" to the group
      "docs.ckan.org-rw"

... using AWS command line tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

(To be done. See e.g. `here <http://andrewhitchcock.org/?post=325>`__)

Set DNS CNAME
-------------

Set a CNAME for the endpoint, e.g. **docs.ckan.org ==>
docs.ckan.org.s3-website-eu-west-1.amazonaws.com**.

How to use the bucket
---------------------

-  Have the "Access Key Id" and the "Secret Access Key" for your user at
   hand
-  Install `s3cmd <http://s3tools.org/s3cmd>`__. For Ubuntu it is
   available as package "s3cmd"
-  Bootstrap a new s3cmd config:

``     s3cmd   -c ~/.s3cfg-docs.ckan.org   --configure``

-  Test:

``     s3cmd   -c ~/.s3cfg-docs.ckan.org   ls   s3://docs.ckan.org/``

-  Now you can do all sorts of thing, e.g. rsync-style

| ``     s3cmd   -c ~/.s3cfg-docs.ckan.org   sync   /path/to/local/clone/   s3://docs.ckan.org/``
| ``     s3cmd   -c ~/.s3cfg-docs.ckan.org   setacl --acl-public   --recursive s3://docs.ckan.org/``

-  TODO: Find out how to sync without having to fix ACLs for public
   access every time

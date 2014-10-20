Domain registration and naming
##############################

Overview
========

Domain names are handled on two, technically independent levels:

-  **Domain Registrar**: To register new domains, to make sure they
   point to the correct DNS servers, and to renew already registered
   domains. We use `gandi.net <http://gandi.net/>`__ (`TLD price
   list <https://www.gandi.net/domain/price/info>`__).
-  **DNS Service**: To administer DNS records (A/CNAME/MX/...) for
   domains which we have already registered. We use `DNS Made
   Easy <http://www.dnsmadeeasy.com/>`__ ("**DME**\ ").

The connection between the two levels is that you have to tell the
registrar which DNS service you intend to use for a specific domain.
These settings are called the **WHOIS NS records**.

Remark: Usually the order doesn't matter, but for some TLDs (e.g. .DE),
a new domain has first to be configured properly at the DNS service
before it can be registered or transferred.


1a: How to *transfer* an *existing* domain to OKF
=================================================

For the current domain owner:
-----------------------------

So you are the current owner of a domain and you want to transfer ot to
OKF. You (or your domain admin) will have to prepare the domain transfer
OR THE TRANSFER IT WILL FAIL. This is what you need to do:

-  Get the **domain (transfer) secret** or **Auth(Info)Code** for the
   domain.
-  Make sure the domain is not locked (some registrar call this
   *"disable Theft protection"*)
-  Make sure any form of owner pseudonymization/anonymisation (*"Privacy
   protection"*) is disabled for the domain. Your mail address as domain
   owner should be visible in the domains `WHOIS
   record <http://www.whois.net/>`__.
-  Make sure that very owner mail address is functional. The registry
   will send a confirmation request to this address.
-  In case the DNS servers for the domain are not already
   ns{10,11,12,13,14,15}.dnsmadeeasy.com. write down the list of all DNS
   records (A, CNAMEs, MX etc) for this domain. This list is called the
   **zone file** for the domain.

When everything is prepared, do this:

-  Send the above !AuthCode and (if required) the zone file to
   sysadmin@okfn.org.
-  Watch mail for the confirmation request from Gandi and follow the
   confirmation link
-  Watch mail for a notification from your current registrar. Usually if
   you don't do anything, the transfer will proceed after a couple of
   days automatically. Sometimes a confirmation link is included which
   allows you to speed up the transfer, but ...

   -  **BEWARE: Some dodgy registrars send transfer notifications
      containing a *"click here to cancel the transfer"* link - DO NOT
      FOLLOW THAT LINK, the transfer would get cancelled immediately
      without any further questions.**.

For the OKF domain admin:
-------------------------

-  Verify the above requirements for domain transfer are given (not
   locked, not anonymized)
-  If the domain is not yet served by the DME name servers: add the
   domains to DME (see below)
-  Log into `gandi.net <http://gandi.net/>`__ with our handle
   OS1535-GANDI and transfer the domain in:

   -  Using the "bulk transfer" option might speed up things
   -  Set our handle OS1535-GANDI (Open Knowledge Foundation,
      sysadmin@okfn.org) as new owner.
   -  Set DNS servers to ns{10,11,12,13,14,15}.dnsmadeeasy.com

-  When the confirmation request to the future owner hits
   sysadmin@okfn.org, confirm it.If a transfer fails, log into Gandi, go
   to our account, *Orders in Progress*, click on *Detail* next to the
   *Failed* message and read the error message carefully. Usually it is
   telling.----

1b: How to *register* a *new* domain for OKF
============================================

This procedure will secure a *formerly non-existing* domain for OKF. And
it will prepare the next step (setting DNS records) by telling the
register what DNS will be used for To register a new domain, log in to the Gandi account, details are in lastpass. Domain registration
is straightforward.

-  Enter the domain name you want to register and click on Search for a
   domain name. This should bring up the results.
-  Make sure the automatically selected domain name has the right
   address (.org or .com etc) and add to cart.
-  Choose 1yr duration and make sure it has the correct contact details
-  Pay using the pre-paid accound.
-  Now set the DNS servers to be the DME name servers:

   -  In the "Domains" dashbord, click on the new domain name
   -  In the bottom right section "dns Name servers", click on "Modify
      servers"
   -  Set the DME name servers:

      -  DNS1=ns10.dnsmadeeasy.com
      -  DNS2=ns11.dnsmadeeasy.com
      -  DNS3=ns12.dnsmadeeasy.com
      -  DNS4=ns13.dnsmadeeasy.com
      -  DNS5=ns14.dnsmadeeasy.com
      -  DNS6=ns15.dnsmadeeasy.com
      -  DNS7= [empty]

   -  Press "Submit"

-  Sign out.

2: How to add a domain to our DNS service
=========================================

To add a new domain (e.g. "okfn.it") to DME's DNS service (which can be
done before or after registration of the domain):

-  Login into https://cp.dnsmadeeasy.com/ as user
   *openknowledgefoundation*. Ask the
   `Sysadmin/sysadmin <Sysadmin/sysadmin>`__ team for the password.
-  In the top right navigation, click on the leftmost "*DNS*\ ", then
   "*Managed DNS*\ ".
-  Click on "Add Domains" and follow the dialog. (**ToDo: document the
   details**)

Note that DME allows so-called "ANAME records". That are "internal
CNAMEs that look from outside like A-records". Very handy!

This is how you can test a DNS record without poisoning your local DNS
cache with old data:

``   host  -t  a  $DOMAIN_NAME_WITH_TAILING_DOT  ns10.dnsmadeeasy.com.``

e.g.

``   host  -t  a  www.bibsoup.net.  ns10.dnsmadeeasy.com.``

You can observe a DNS record until you see the change:

``   watch  host  -t  a  $DOMAIN_NAME_WITH_TAILING_DOT  ns10.dnsmadeeasy.com.``

Now the change should show up in the global DNS system (provided the
domain was properly delegated at its registrar to the DME DNS servers).
If your local DNS cache isn't storing an outdated record, you should see
the change now:

``   host  -t  a  $DOMAIN_NAME_WITH_TAILING_DOT``

Alternatively you could verify that "ping" is now using the correct IP
address:

``   ping  $DOMAIN_NAME``

Remarks:

-  '''Always \*test\* DNS changes as described above! '''
-  Do not forget tailing dots when you create CNAMEs, otherwise e.g.
   www.bibsoup.net. might end up pointing to
   s065.okserver.org.bibsoup.net. instead of s065.okserver.org.

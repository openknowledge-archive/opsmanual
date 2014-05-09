Email aliases and forwarders
############################

Email aliases and forwarding is managed on fastmail.fm.

How to add a new domain
-----------------------

Set the MX records to the new domain as below::

    20    in2-smtp.messagingengine.com.
    10    in1-smtp.messagingengine.com.

Now add the new domain in the `FastMail Virtual Domains configuration page
<fm-config_>`_ (log in from LastPass)

   .. _fm-config: https://www.fastmail.fm/html/?MSS=!SE-*&MSignal=VD-*&u=a864411d

The configuration in fastmail should look like below.

   .. image:: email-fastmail-domain.jpg

If you cannot set Routing to Auto (Int) at the time of creation, submit the
form and you will be able to change it.

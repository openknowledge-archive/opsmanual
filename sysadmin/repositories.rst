Repositories
############

We attempt, as far as is reasonably possible given the tools and time available,
to abide by the principle of "Infrastructure as Code" -- all parts of our
infrastructure configuration should be easily reproducible in an automated
fashion.

The following repositories are used to manage the OKF infrastructure:

``infra``
---------

https://github.com/okfn/infra is the main repository we use to manage our
infrastructure. It includes configuration and data for the Ansible_
configuration management system.

.. _Ansible: http://www.ansibleworks.com/

``credentials``
---------------

https://github.com/okfn/credentials is a private repository containing secrets,
keys, and other things the rest of the world shouldn't know. The contents of
this repository are encrypted using git-crypt_.

.. _git-crypt: https://www.agwa.name/projects/git-crypt/

``sysadmin-oz``
---------------

https://github.com/okfn/sysadmin-oz contains a simple tool to optimize DNS zones
according to our internal DNS standards.

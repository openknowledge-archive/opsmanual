Monitoring and Graphs
=====================

We use a bunch of tools for monitoring and graphing - which are
`Nagios`_, `check\_mk`_, `graphite`_, `collectd`_.

Each of these are managed as a role in the Ansible `OKF infra repo`_

Host and service monitoring
---------------------------

`Nagios <http://nagios.okfn.org>`__ is setup to monitor hosts and
services.

check\_mk is used along with nagios, since check\_mk allows managing
nagios config as a template. Checks are mainly passive, thus reducing
load on the nagios server. The check\_mk agent is installed on each
host, which adds passive checks for host services.

All nagios config is managed using Ansible from within the
`check-mk-server`_ and `nagios-server`_ roles.

Add a host into Nagios
^^^^^^^^^^^^^^^^^^^^^^

To add a host for monitoring, the host just needs to be listed under the
right group in the Ansible Inventory.  Which is either ckan, openspending 
or sysadmin-server.  Normally add the server in to it's own relevant group 
and then add that group into the sysadmin-server, ckan or openspending, 
depending as to what's needed.

``https://github.com/okfn/infra/blob/master/ansible/inventory/hosts``

Once the host is added into the inventory, running the check\_mk-server
play on the target host, which should take care of the rest.

``ansible-playbook  main.yml --tags="check_mk_config_main" -i inventory/hosts -c ssh -vvv``

Remove a host from Nagios
^^^^^^^^^^^^^^^^^^^^^^^^^

To remove a host from nagios, it must be removed from the Ansible
`inventory`_.

We also need to remove any .yml `vars`_ file for the host.

The next step is to update the check\_mk config, by running:

``ansible-playbook  main.yml --tags="check_mk_config_main" -i inventory/hosts -c ssh -vvv``

Other monitoring related Ansible flags
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We have a bunch of flags in Ansible to simplify managing checks for each host,
they are documented here in the `OKF infra repo
<https://github.com/okfn/infra/tree/master/ansible/inventory>`__

Stats collection and graphs in Graphite
---------------------------------------

Graphite is used to aggregate and display stats as graphs, stats are gathered
with with collectd.

-  Resource graphs for OKF servers are at `graphite.okfn.org`_.
-  The OKF infra ansible ``monitoring`` role is used to add servers into
   graphite.
-  Collectd is installed on every host using the ``monitoring`` role

Our current Dashboards
^^^^^^^^^^^^^^^^^^^^^^

-  `All system Metrics`_
-  `Mail Metrics`_
-  `Openspending Metrics`_

.. _Nagios: http://nagios.org
.. _check\_mk: http://mathias-kettner.com/check_mk_introduction.html
.. _graphite: http://graphite.wikidot.com/
.. _collectd: http://collectd.org
.. _OKF infra repo: https://github.com/okfn/infra/tree/master/ansible/roles
.. _check\_mk-server: https://github.com/okfn/infra/tree/master/ansible/roles/check_mk-server
.. _nagios-server: https://github.com/okfn/infra/tree/master/ansible/roles/check_mk-server
.. _inventory: https://github.com/okfn/infra/tree/master/ansible/inventory
.. _vars: https://github.com/okfn/infra/tree/master/ansible/inventory/host_vars
.. _graphite.okfn.org: http://graphite.okfn.org/
.. _All system Metrics: http://graphite.okfn.org/dashboard#system-metrics
.. _Mail Metrics: http://graphite.okfn.org/dashboard#mail-metrics
.. _Openspending Metrics: http://graphite.okfn.org/dashboard#openspending-application-metrics

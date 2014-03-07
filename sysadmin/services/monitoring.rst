# Monitoring and Graphs

We use a bunch of tools for monitoring and graphing - which are [Nagios](http://nagios.org), [check_mk](http://mathias-kettner.com/check_mk_introduction.html), [graphite](http://graphite.wikidot.com/), [collectd](http://collectd.org).<br>

Each of these are managed as a role in the Ansible [OKF infra repo](https://github.com/okfn/infra/tree/master/ansible/roles)


## Host and service monitoring

[Nagios](http://nagios.okfn.org) is setup to monitor hosts and services.

check_mk is used along with nagios, since check_mk allows managing nagios config as a template.<br>
Checks are mainly passive, thus reducing load on the nagios server.<br>
The check_mk agent is installed on each host, which adds passive checks for host services.

All nagios config is managed using Ansible from within the [check_mk-server](https://github.com/okfn/infra/tree/master/ansible/roles/check_mk-server) and [nagios-server](https://github.com/okfn/infra/tree/master/ansible/roles/check_mk-server) roles.


#### Add a host into Nagios

<br>

To add a host for monitoring, the host just needs to be listed under the right group in the Ansible Inventory.

`https://github.com/okfn/infra/blob/master/ansible/inventory/hosts`

Once the host is added into the inventory, running the check_mk-server play on the target host,<br> 
which should take care of the rest. <br>

`ansible-playbook  main.yml --tags="check_mk_config_main" -i inventory/hosts -c ssh -vvv`

#### Remove a host from Nagios

To remove a host from nagios, it must be removed from the Ansible [inventory](https://github.com/okfn/infra/tree/master/ansible/inventory).

We also need to remove any .yml [vars](https://github.com/okfn/infra/tree/master/ansible/inventory/host_vars) file for the host.

The next step is to update the check_mk config, by running:

`ansible-playbook  main.yml --tags="check_mk_config_main" -i inventory/hosts -c ssh -vvv`


#### Other monitoring related Ansible flags
<br>
We have a bunch of flags in Ansible to simplify managing checks for each host, <br>
they are documented here in the [OKF infra repo](https://github.com/okfn/infra/tree/master/ansible/inventory)

## Stats collection and graphs in Graphite
<br>
Graphite is used to aggregate and display stats as graphs, stats are gathered with with collectd.

- Resource graphs for OKF servers are at [graphite.okfn.org](http://graphite.okfn.org/). 
- The OKF infra ansible `monitoring` role is used to add servers into graphite. 
- Collectd is installed on every host using the `monitoring` role 

#### Our current Dashboards

- [All system Metrics](http://graphite.okfn.org/dashboard#system-metrics)
- [Mail Metrics](http://graphite.okfn.org/dashboard#mail-metrics)
- [Openspending Metrics](http://graphite.okfn.org/dashboard#openspending-application-metrics)


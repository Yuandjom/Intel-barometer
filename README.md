Barometer
---------
- This work is licensed under a Creative Commons Attribution 4.0 International License.
- http://creativecommons.org/licenses/by/4.0

```
    Note: this repository provides a demo implementation. It is not intended
    for unmodified use in production. It has not been tested for production.
```


The ability to monitor the Network Function Virtualization Infrastructure
(NFVI) where VNFs are in operation will be a key part of Service Assurance
within an NFV environment, in order to enforce SLAs or to detect violations,
faults or degradation in the performance of NFVI resources so that events
and relevant metrics are reported to higher level fault management systems.
If fixed function appliances are going to be replaced by virtualized
appliances the service levels, manageability and service assurance needs
to remain consistent or improve on what is available today.

As such, the NFVI needs to support the ability to monitor:

#. Traffic monitoring and performance monitoring of the components that
   provide networking functionality to the VNF, including: physical
   interfaces, virtual switch interfaces and flows, as well as the
   virtual interfaces themselves and their status, etc.
#. Platform monitoring including: CPU, memory, load, cache, thermals, fan
   speeds, voltages and machine check exceptions, etc.


All of the statistics and events gathered must be collected in-service and
must be capable of being reported by standard Telco mechanisms (e.g. SNMP,
REST), for potential enforcement or correction actions. In addition, this
information could be fed to analytics systems to enable failure prediction,
and can also be used for intelligent workload placement.


## How to run this repository 
```
git clone https://github.com/Yuandjom/Intel-barometer.git
```

## Prerequitises
```
sudo apt install python3-pip
sudo pip install ansible
```

After clone the repository check the version of ansible 
```
Ansible: 9.0
Jinja 3.0 (note that this has to be <3.1, pip install jinja2<3.1)
```

Run the ansible script 
```
cd /barometer/docker/ansible
sudo ansible-playbook -i default.inv collectd_service.yml
```

Problem:
```
root@spr22:/home/Ayush/barometer/docker/ansible# docker logs bar-collectd
[2023-11-23 03:29:32] plugin_load: plugin "logfile" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "capabilities" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "contextswitch" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "cpu" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "cpufreq" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "csv" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "df" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "disk" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "dpdk_telemetry" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "ethstat" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "exec" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "hugepages" successfully loaded.
[2023-11-23 03:29:32] plugin_load: Could not find plugin "intel_pmu" in /opt/collectd/lib/collectd
[2023-11-23 03:29:32] There is configuration for the `intel_pmu' plugin, but the plugin isn't loaded. Please check your configuration.
[2023-11-23 03:29:32] plugin_load: plugin "intel_rdt" successfully loaded.
[2023-11-23 03:29:32] intel_rdt: No core groups configured. Default core groups created.
[2023-11-23 03:29:32] plugin_load: plugin "ipc" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "ipmi" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "irq" successfully loaded.
[2023-11-23 03:29:32] plugin_load: plugin "load" successfully loaded.
Warning: Failed to connect to the agentx master agent ([NIL]): 
Error: Parsing the config file failed!
```


# Troubleshooting
```
fatal: [192.168.2.67]: FAILED! => {"msg": "template error while templating string: cannot import name 'environmentfilter' from 'jinja2.filters' (/usr/local/lib/python3.10/dist-packages/jinja2/filters.py)\n  line 0. String: {{ collectd_plugins | union(['capabilities']) | unique }}"}
```
Solution: 
https://github.com/ansible/ansible/issues/77413
```
pip install jinja2<3.1
```
Ansible script unable to run collectd container

Solution
```
cd barometer

ansible-galaxy install -r requirements.yml
```

```

```

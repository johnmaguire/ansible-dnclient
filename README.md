Ansible Role: `johnmaguire.dnclient`
=========

Installs [dnclient](https://defined.net) on some servers.

Requirements
------------

None

Role Variables
--------------

You can override the dnclient download URL with the following variable:

```
dnclient_latest_url: "https://dl.defined.net/02c6d0f9/v0.1.9/linux/amd64/dnclient"
```

Otherwise the latest version will be downloaded.

For hosts running the ufw firewall, you can allow dnclient traffic with the following variable:

```
dnclient_network_cidr: "192.168.123.0/24"
```

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: johnmaguire.dnclient }

License
-------

MIT

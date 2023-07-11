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

By default, dnclient will be installed to `/opt/defined/dnclient`. You can change the install directory with the following variable:

```
dnclient_install_dir: "/usr/local/bin"
```

You can enroll the host with the following variables:

```
dnclient_enroll: true
dnclient_api_key: "dnkey-AAAAAAAAAAAAAAAAAAAAAAAAAA-0000000000000000000000000000000000000000000000000000" # store this using ansible-vault or similar
```

Optionally, you can configure the enrolled host using:

```
dnclient_hostname: "lighthouse1"
dnclient_network_id: "network-AAAAAAAAAAAAAAAAAAAAAAAAAA"
dnclient_ip_address: "192.168.123.1"
dnclient_role_id: "role-AAAAAAAAAAAAAAAAAAAAAAAAAA"
dnclient_static_addresses: ["lighthouse1.example.com:4242"]
dnclient_listen_port: 4242
dnclient_is_lighthouse: true
dnclient_is_relay: true
```

For hosts running the ufw firewall, you can allow dnclient traffic with the following variables:

```
dnclient_firewall_exception: true
dnclient_network_cidr: "192.168.123.0/24" # auto-detected if dnclient_api_key is set
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

Ansible Role: `johnmaguire.dnclient`
=========

Installs [dnclient](https://defined.net) on some servers.

Installation Method
-------------------

On Debian and Ubuntu hosts, dnclient is installed from the [Defined Networking
apt repository](https://dl.defined.net/stable/apt) (`dnclient` package), so
updates flow through normal `apt` upgrades.

On all other platforms (e.g. Arch Linux, macOS) the binary is downloaded from
the Defined Networking downloads API and the service is installed manually, as
no package is published for them.

Requirements
------------

None

Role Variables
--------------

For platforms that download the binary (non-Debian), you can override the
download URL with the following variable:

```
dnclient_latest_url: "https://dl.defined.net/02c6d0f9/v0.1.9/linux/amd64/dnclient"
```

Otherwise the latest version will be downloaded.

For these platforms, dnclient will be installed to `/opt/defined/dnclient` by
default. You can change the install directory with the following variable
(this has no effect on apt-managed hosts, where the package installs to
`/usr/bin/dnclient`):

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
dnclient_tags: ["env:prod"]
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

Ansible Role: `johnmaguire.dnclient`
=========

Installs [dnclient](https://defined.net) on some servers.

Installation Method
-------------------

On Debian and Ubuntu hosts, dnclient is installed from the [Defined Networking
apt repository](https://dl.defined.net/stable/apt) (`dnclient` package), and on
RHEL/Fedora hosts from the [rpm repository](https://dl.defined.net/stable/rpm),
so updates flow through normal `apt`/`dnf` upgrades.

On Arch Linux hosts, dnclient is installed from the
[AUR](https://aur.archlinux.org/packages/dnclient) using `yay` via the
`kewlfft.aur` collection.

On all other platforms (e.g. macOS) the binary is downloaded from the Defined
Networking downloads API and the service is installed manually, as no package
is published for them.

Requirements
------------

On Arch Linux hosts, the `kewlfft.aur` collection must be installed on the
controller, and the host must have `yay` and an unprivileged build user
(`dnclient_aur_builder_user`, default `builduser`) that can run `pacman` via
passwordless sudo.

Role Variables
--------------

For platforms that download the binary (e.g. macOS), you can override the
download URL with the following variable:

```
dnclient_latest_url: "https://dl.defined.net/02c6d0f9/v0.1.9/linux/amd64/dnclient"
```

Otherwise the latest version will be downloaded.

For these platforms, dnclient will be installed to `/opt/defined/dnclient` by
default. You can change the install directory with the following variable
(this has no effect on package-managed hosts, where the package installs to
`/usr/bin/dnclient`):

```
dnclient_install_dir: "/usr/local/bin"
```

Custom Builds
-------------

To run a locally built dnclient instead of a release, point
`dnclient_custom_binary` at it on the controller:

```
dnclient_custom_binary: "/home/me/src/dnclient/build/linux-amd64/dnclient"
```

It is evaluated per host, so it can be templated by platform:

```
dnclient_custom_binary: "/home/me/src/dnclient/build/{{ dnclient_os }}-{{ dnclient_arch }}/dnclient"
```

The binary is pushed to `dnclient_custom_bin` (`/usr/local/bin/dnclient`) and
executed there to confirm it runs on the host before anything is switched over
to it — a binary built for the wrong OS or architecture fails the play rather
than the tunnel. On package-managed hosts a systemd drop-in overrides the
unit's `ExecStart`, leaving the package installed and untouched; on platforms
that install the binary directly (macOS) it replaces the downloaded one.

Unsetting `dnclient_custom_binary` and re-running removes the binary and the
drop-in, putting the host back on the released build.

Each run also compares the binary the service is actually executing against the
intended one and restarts if they differ, so a host that lost its restart to a
dropped connection converges on the next run rather than sitting with a correct
drop-in and the old binary still running.

On Arch Linux hosts, you can change the unprivileged user the AUR build runs
as (see Requirements):

```
dnclient_aur_builder_user: "builduser"
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
dnclient_network_cidrs: ["192.168.123.0/24", "fd00:1234::/80"] # auto-detected if dnclient_api_key is set
```

Dependencies
------------

The `kewlfft.aur` collection, on Arch Linux hosts only.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: johnmaguire.dnclient }

License
-------

MIT

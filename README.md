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

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

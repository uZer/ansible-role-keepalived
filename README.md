ansible-role-keepalived
=======================
[![Build Status](https://travis-ci.org/uZer/ansible-role-keepalived.svg?branch=master)](https://travis-ci.org/uZer/ansible-role-keepalived)

This role installs keepalived from apt/yum repository.
Can setup multiple service checks and multiple VIP.

All variables should be configured in `host_vars` or `group_vars`.

Dependencies
------------
Should work on Centos, Redhat, Ubuntu, Debian.

Exhaustive usage example
------------------------

* Host variables (should be defined in `host_vars/...`):

```yaml
keepalived_role:
  "VI_1": "MASTER"              # Can be MASTER or SLAVE
  "VI_2": "MASTER"
  "VI_3": "SLAVE"
```

* Group variables (should be defined in `group_vars/...` per keepalived pod):


```yaml
keepalived_vrrp_instances:
  "VI_1":
    virtual_router_id: 51       # Will be used as interface label
    master_priority: 50         # Priority. The smaller, the greater
    slave_priority: 100
    shared_iface: eth0
    shared_vips:
      - '10.127.50.3'
    auth_pass: "{{ vault_vrrp_passwords["VI_1"] }}"

  "VI_2":
    virtual_router_id: 52
    master_priority: 50
    slave_priority: 100
    shared_iface: eth0
    shared_vips:
      - '10.127.50.4'
    auth_pass: "{{ vault_vrrp_passwords["VI_2"] }}"

  "VI_3":
    virtual_router_id: 53
    master_priority: 50
    slave_priority: 100
    shared_iface: eth0
    shared_vips:
      - '10.127.50.5'
    auth_pass: "{{ vault_vrrp_passwords["VI_3"] }}"
    checks:
     - chk_nginx
     - chk_keepalived

keepalived_checks:
  "chk_nginx":
    process: nginx              # Monitor NGINX
    period: 2                   # every 2 seconds
  "chk_keepalived":
    process: keepalived         # Monitor Keepalived
    period: 2                   # every 2 seconds

# In vault:
vault_keepalived_vrrp_passwords:
    "VI_1": 'YUiszRrpLEXbnMLqDapwWLvPAJ'
    "VI_2": 'rJEUkQnmSRahwRTpjUkFVid'
    "VI_3": 'itkcbxmaXwFBiUZJhePmtX'

# OPTIONAL FOR NOTIFICATION AND TUNING
keepalived_advert_interval: 1
keepalived_notify_from: "keepalived@{{ ansible_fqdn }}"
keepalived_notify_smtp_server: "smtp.example.com"
keepalived_notify_smtp_timeout: "30"
keepalived_notify_list:
  - "noc@example.com"
  - "abuse@example.com"
```

Minimum usage example
---------------------
You should at least define these variables:

```yaml
# Per host
keepalived_role:
  "VI_1": "MASTER"

# Per group
keepalived_vrrp_instances:
  "VI_1":
    virtual_router_id: 10
    shared_iface: eth0
    shared_vips:
      - '10.127.50.5'
    auth_pass: 'NLxQZYzXUfhjkJqWzVrPMua'
```

License
-------
"THE (extended) BEER-WARE LICENSE" (Revision 42.0815):

As long as you retain this notice you can do whatever you want with this stuff.
If we meet some day, and you think this stuff is worth it, you can buy me some
beers in return.


Author Information
------------------
Youenn Piolet

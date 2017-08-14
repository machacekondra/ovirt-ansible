VDSM configuration
==================

The `vdsm-conf` role manages the configuration of the VDSM service of the oVirt hosts.

Requirements
------------

 * Ansible version 2.3 or higher
 * Python SDK version 4 or higher

Role Variables
--------------

| Name               | Default value     |                                              |
|--------------------|-------------------|----------------------------------------------| 
| cluster_name       | UNDEF             | All hosts in this cluster will have changed VDSM configuration. |
| host_names         | UNDEF             | List of host names to change VDSM configuration. |
| vdsm_configuration | UNDEF             | List of VDSM configuration options. See the structure below.  |
| configuration_backup | UNDEF           | Create a backup file including the timestamp information so you can get the original file back if you somehow clobbered it incorrectly. |
| wait_for_host_up_state | true          | If true wait for host to appear UP in engine after configuration change. |
| vdsm_file_path     | /etc/vdsm/vdsm.conf | Path to INI like file configuration to modify. |
| host_statuses      | [UP]              | List of host statuses which will be used to VDSM configuration modification. |

The `vdsm_configuration` list can contain following attributes:

| Name       | Default value         |                                         |
|------------|-----------------------|-----------------------------------------| 
| section    | vars                  | Name of the VDSM configuration section. |
| option     | UNDEF                 | Name of the option to be changed.       |
| value      | UNDEF                 | Value of the option to be set.          |

Dependencies
------------

No.

Example Playbook
----------------

__IMPORTANT__: When executing the playbook you need to specify also
credentials to hosts you want to change the VDSM configuration. One
possible way is to use private key as follows:

```bash
$ ansible-playbook --private-key=/etc/pki/ovirt-engine/keys/engine_id_rsa examples/vdsm_conf.yml
```

Or you can use `ansible_pass` variable for SSH root connection password.

```yaml
---
- name: oVirt infra
  hosts: localhost
  connection: local
  gather_facts: false

  vars:
    engine_url: https://ovirt-engine.example.com/ovirt-engine/api
    engine_user: admin@internal
    engine_password: 123456
    engine_cafile: /etc/pki/ovirt-engine/ca.pem

    host_names:
      - west1
      - west2
      - east1
      - east2
    
    vdsm_configuration:
      - section: vars
        option: debug
        value: true

  roles:
    - ovirt-vm-infra
```

License
-------

Apache License 2.0

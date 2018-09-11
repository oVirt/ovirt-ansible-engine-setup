oVirt Engine Setup
==================

Install required packages for oVirt Engine deployment, generates answerfile
and runs engine-setup.
Optionally the role update oVirt engine packages.

Target Systems
--------------

* engine

Requirements
------------

 * Environment with configured repositories.
 * Ansible version 2.5

Role Variables
--------------

By default engine-setup uses answer file specific for version of oVirt,
based on ``ovirt_engine_setup_version`` parameter. You can specify own answer file
as ``ovirt_engine_setup_answer_file_path``.

* Common options:

| Name                            | Default value         |  Description                                              |
|---------------------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_setup_version            | 4.2                   | Allowed versions: [4.1, 4.2] |
| ovirt_engine_setup_product_type       | oVirt                 | One of ["oVirt", "RHV"], case insensitive. |
| ovirt_engine_setup_package_list       | []                    | List of extra packages to be installed on engine apart from ovirt-engine package. |
| ovirt_engine_setup_answer_file_path   | UNDEF                 | Path to custom answerfile for `engine-setup`. |
| ovirt_engine_setup_use_remote_answer_file | false             | If `True`, use answerfile's path on the remote machine. This option should be used if the installation occure on the remote machine and the answerfile is located on there also. |
| ovirt_engine_setup_organization       | UNDEF                 | Organization name for certificate. |
| ovirt_engine_setup_firewall_manager   | firewalld             | Specify the type of firewall manager to configure on Engine host, following values are availableL: `firewalld`,`iptables` or empty value to skip firewall configuration. |
| ovirt_engine_setup_update_setup_packages | False              | If `True`, setup packages will be updated before `engine-setup` will be executed. Makes sense if Engine is already installed. |
| ovirt_engine_setup_update_all_packages | True                 | If `True`, all packages will be updated before `engine-setup` will be executed. |
| ovirt_engine_setup_offline            | False                 | If `True`, `engine-setup` will not search for package updates. |
| ovirt_engine_setup_accept_defaults    | False                 | If `True` default answers will be automatically used in questions that have them during `engine-setup`. |
| ovirt_engine_setup_require_rollback   | UNDEF                 | If `True` setup will require to be able to rollback new packages in case of a failure. If not passed the default answer from `engine-setup` will be chosen. Valid for updating/upgrading. |
| ovirt_engine_setup_admin_password     | UNDEF                 | Password for the automatically created administrative user of the oVirt Engine.

* Engine Database:

| Name                            | Default value         |  Description                                              |
|---------------------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_setup_db_host            | localhost             | IP address or host name of a PostgreSQL server for Engine database. By default the database will be configured on the same host as Engine. |
| ovirt_engine_setup_db_port            | 5432                  | Engine database port. |
| ovirt_engine_setup_db_name            | engine                | Engine database name. |
| ovirt_engine_setup_db_user            | engine                | Engine database user. |
| ovirt_engine_setup_db_password        | UNDEF                 | Engine database password. |
| ovirt_engine_setup_engine_vacuum_full | False                 | Used only when upgrading. If `True`, before upgrade it will vacuum engine database |

* Engine Data Warehouse Database:

| Name                            | Default value         |  Description                                              |
|---------------------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_setup_dwh_db_configure   | True            | If `True` the DWH Database will be configured manually. |
| ovirt_engine_setup_dwh_db_host        | localhost             | IP address or host name of a PostgreSQL server for DWH database. By default the DWH database will be configured on the same host as Engine. |
| ovirt_engine_setup_dwh_db_port        | 5432                  | DWH database port. |
| ovirt_engine_setup_dwh_db_name        | ovirt_engine_history  | DWH database name. |
| ovirt_engine_setup_dwh_db_user        | ovirt_engine_history  | DWH database user. |
| ovirt_engine_setup_dwh_db_password    | UNDEF                 | DWH database password. |
| ovirt_engine_setup_dwh_vacuum_full    | False                 | Used only when upgrading. If `True`, before upgrade it will vacuum dwh database |

* OVN related options:

| Name                            | Default value         |  Description                                              |
|---------------------------------|-----------------------|-----------------------------------------------------------|
| ovirt_engine_setup_provider_ovn_configure| true               | If `True` OVN provider will be configured. Valid for `ovirt_engine_setup_version` >= 4.2 |
| ovirt_engine_setup_provider_ovn_username | admin@internal     | Username for OVN. |
| ovirt_engine_setup_provider_ovn_password | UNDEF              | Password for OVN. |


Dependencies
------------

None

Example Playbook
----------------

```yaml
---
# Example of oVirt setup:
- name: Setup oVirt
  hosts: engine
  vars_files:
    # Contains encrypted `ovirt_engine_setup_admin_password` variable using ansible-vault
    - passwords.yml
  vars:
    ovirt_engine_setup_version: '4.2'
    ovirt_engine_setup_organization: 'of.ovirt.engine.com'
  roles:
    - oVirt.engine-setup

# Example of RHV setup:
- name: Setup RHV
  hosts: engine
  vars_files:
    # Contains encrypted `ovirt_engine_setup_admin_password` variable using ansible-vault
    - passwords.yml
  vars:
    ovirt_engine_setup_version: '4.2'
    ovirt_engine_setup_organization: 'rhv.redhat.com'
    ovirt_engine_setup_product_type: 'rhv'
  roles:
    - oVirt.engine-setup
```


Ansible Role: KeyDB Cluster
=========

![CI](https://github.com/v0112358/ansible-role-keydb-cluster/actions/workflows/main.yml/badge.svg) ![Ansible Role](https://img.shields.io/ansible/role/d/55902) [![GitHub license](https://img.shields.io/github/license/v0112358/ansible-role-keydb-cluster)](https://github.com/v0112358/ansible-role-keydb-cluster/blob/master/LICENSE.md)

Install and configure KeyDB cluster on your system.

Example Inventory
------------
```
[keydb_cluster:children]
keydb_cluster_infra

[keydb_cluster_infra]
vm-dev-keydb-infra-0	keydb_role="master"
vm-dev-keydb-infra-1	keydb_role="master"
vm-dev-keydb-infra-2	keydb_role="master"
vm-dev-keydb-infra-3	keydb_role="slave"
vm-dev-keydb-infra-4	keydb_role="slave"
vm-dev-keydb-infra-5	keydb_role="slave"
```
Example Playbook
------------

```
---
- name: Deploy KeyDB Cluster
  hosts: keydb_cluster
  pre_tasks:
    - name: Verify Ansible meets KeyDB cluster requirements.
      assert:
        that: "ansible_version.full is version_compare('2.10.0', '>=')"
        msg: >
          "You must update Ansible to at least 2.10.0 to use this playbook"
  roles:
    - { role: ansible-role-keydb-cluster, tags: keydb-cluster }
```

Role Variables
--------------

These variables are set in defaults/main.yml.
```
---
keydb_packages:
  rpm: 
    - https://download.keydb.dev/pkg/open_source/rpm/centos{{ ansible_distribution_major_version }}/x86_64/keydb-latest-1.el{{ ansible_distribution_major_version }}.x86_64.rpm
  ppa:
    apt_repository: https://download.keydb.dev/open-source-dist
    apt_key: https://download.keydb.dev/open-source-dist/keyring.gpg


keydb_cluster_replica: 1
keydb_cluster_conf:
  cluster_enabled: "yes"
  master_port: "6379"
  slave_port: "6379"
  maxmemory: "64mb"
  rename_commands:
    - FLUSHDB
    - FLUSHALL
    - KEYS
    - SHUTDOWN
....
```

Requirements
------------

pip packages listed in requirements.txt.

License
-------

MIT

Author Information
------------------
v0112358
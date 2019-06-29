Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Inventory
----------------

```
[guacamole]
18.205.67.151 ansible_host=18.205.67.151 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/mford-useast-1-key.pem

```

Example Playbook
----------------

```
---
- name: Populate Apache Guacamole with Users and Connections
  hosts: guacamole
  gather_facts: False
  vars_files:
    - ./vars/default_vars.yml
    - ./vars/guacamole_credentials.yml

  tasks:

    - name: Set guacamole_server fact
      set_fact:
        guacamole_server: "{{ ansible_host }}"

    - name: Invoke populate_guacamole role
      include_role:
        name: populate_guacamole
      with_sequence: start=1 count="{{ student_total }}"

    - name: Clean Up JSON Files
      file:
        path: "{{ playbook_dir }}/{{ item }}"
        state: absent
      with_items:
        - rdp_connection.json
        - ssh_connection.json
        - users.json
      delegate_to: localhost
```
License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

Ansible Role: install-requirements
=========

install, remove and update python and/or os packages.

Requirements
------------

installs role and all of it's dependencies w/:

<details><summary>Install role</summary>

```
cat <<EOF > /tmp/requirements.yaml
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/install-requirements.git
  scm: git
EOF
ansible-galaxy install -r /tmp/requirements.yaml --force
rm -rf /tmp/requirements.yaml
```
</details>

Example Playbook
----------------

This playbook install the htop os package and the python module kubernetes with the version 10.0.1 and the latest openshift python module.

<details><summary>Example</summary>
<br/>
Playbook: install-requirements.yml

```
---
- hosts: localhost
  gather_facts: true
  become: true
  vars:
    update_packages: true
    os_packages:
      - htop
    python_modules:
      - name: kubernetes
        version: 10.0.1
      - name: openshift
  
  roles:
   - install-requirements
```

Playbook execution:
```
ansible-playbook -i inventory install-reqierements.yml
```
</details>

Role Variables
--------------

The following vars can be set:

<details><summary>vars-examples</summary>
<br/>

```
vars:
  update_packages: true     # set for update or not update your os packages (update_packages: true/ false)
  os_packages:
    - htop  
    - unzip # the os package that you want to install. If not set, no os package will be installed. (os_packages: <package_name>)
  python_modules:
    - name: kubernetes      # the pip package that you want to install. If not set, no os package will be installed. If pip doesn't exist, it will be installed automatically. The pip version is decided based on the python version that is used by ansible on the target host.
      version: 10.0.1       # - python_modules: <package_name>
    - name: openshift       #   version: <package_version>
    
```
</details>

## Role Version:
| date  | who | changelog |
|---|---|---|
|2020-08-24   | Patrick Hermann  | fixed task names, fixed os task loop
|2020-08-18   | Marcel Zapf   | initial commit, added tests and readme


License
-------

BSD

Author Information
------------------

Marcel Zapf (marcel.zapf@sva.de; SVA GmbH; 08/2020);
Patrick Hermann (patrick.hermann@sva.de; SVA GmbH; 08/2020)


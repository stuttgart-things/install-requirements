Ansible Role: install-requirements
=========

This Ansible role can install, remove and update python and os packages.

Requirements
------------

<details><summary>Install role</summary>

installs this role and all it's dependencies w/:
```
cat <<EOF > /tmp/requirements.yaml
- src: git@codehub.sva.de:Lab/stuttgart-things/virtual-machines/create-packer-vmtemplate.git
  scm: git

EOF
ansible-galaxy install -r /tmp/requirements.yaml --force
rm -rf /tmp/requirements.yaml
```
</details>
<br/>

Role Variables
--------------

This role use ansible variables. 
```
update_packages: true/ false
```
Set for update or not update your os packages.
```
- os_packages: <package_name>
```
The os package that you want to install. If not set, no os package will be installed
```
- python_modules: <package_name>
  version: <package_version>
```
The pip package that you want to install. If not set, no os package will be installed. If pip doesn't exist, it will be installed automatically. The pip version is decided based on the python version that is used by ansible on the target host.

Example Playbook
----------------

<details><summary>Example</summary>
<br/>
Playbook: ./tests/test.yml

```
---
- hosts: localhost
  gather_facts: true
  become: true
  vars:
    update_packages: true
    os_packages:
      - name: htop
    python_modules:
      - name: kubernetes
#        version: 10.0.1
      - name: openshift
  
  roles:
   - install-requirements
```


Playbook execution:
```
ansible-playbook -i inventory install-reqierements
```
</details>
<br/>

License
-------

BSD

Author Information
------------------

Marcel Zapf (marcel.zapf@sva.de; SVA GmbH; 08/2020)

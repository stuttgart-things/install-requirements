stuttgart-things/install-requirements
====================

install, remove and update python and/or os packages.

<details><summary>VARIABLES</summary>

The following vars can be set:
```yaml
vars:
  update_packages: true     # set for update or not update your os packages (update_packages: true/ false)
  os_packages:
    - htop
    - unzip # the os package that you want to install. If not set, no os package will be installed. (os_packages: <package_name>)
  python_modules:
    - name: kubernetes      # the pip package that you want to install. If not set, no os package will be installed. If pip doesn't exist, it will be installed automatically. The pip version is decided based on the python version that is used by ansible on the target host.
      version: 10.0.1       # - python_modules: <package_name>
    - name: openshift       #   version: <package_version>
  ansible_collections:
    podman:
      name: containers.podman
      version: 1.3.1
    general:
      name: community.general
      version: 1.2.0
    crypto:
      name: community.crypto
      version: 1.2.0
```

</details>

<details><summary>DEFAULTS</summary>

```yaml
...
packer_update_packages: false

packer_python_modules:
  - name: netaddr

packer_os_prerequisites:
  - unzip
  - git
  - ansible
...

```

</details>

<details><summary>ROLE INSTALLATION</summary>

```bash
cat <<EOF > /tmp/requirements.yaml
- src: https://github.com/stuttgart-things/install-requirements.git
  scm: git
EOF

ansible-galaxy install -r /tmp/requirements.yaml --force
rm -rf /tmp/requirements.yaml
```

</details>

<details><summary>EXAMPLE INVENTORY</summary>

```bash
cat <<EOF > inventory
[appserver]
1.2.3.4 ansible_user=sthings
EOF
```

</details>

<details><summary>EXAMPLE PLAYBOOK</summary>

This playbook install the htop os package and the python module kubernetes with the version 10.0.1 and the latest openshift python module.

```yaml
cat <<EOF > install-requirements.yaml
---
- name: Install packages
  hosts: localhost
  gather_facts: true
  become: true
  vars:
    update_packages: true
    os_packages:
      - htop
      - unzip
    python_modules:
      - name: kubernetes
        version: 10.0.1
      - name: openshift

  roles:
    - install-requirements
EOF
```

</details>

<details><summary>EXAMPLE EXECUTION</summary>

```bash
ansible-playbook -i inventory install-requirements.yml
```

</details>

<details><summary>EXAMPLE ROLE INCLUDE OF ANOTHER ROLE</summary>

```yaml
# task file
...
- name: Install prerequisites
  include_role:
    name: install-requirements
  vars:
    update_packages: "{{ packer_update_packages }}"
    os_packages: "{{ packer_os_prerequisites }}"
    python_modules: "{{ packer_python_modules }}"
  tags: setup
...
```

</details>

<details><summary>LICENSE</summary>

Copyright 2020 patrick hermann.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

</details>

## Role history

| date  | who | changelog |
|---|---|---|
|2024-05-07   | Andre Ebert | linting and testing on different OS
|2020-10-21   | Marcel Zapf | fixed problem debian system not update os packages
|2020-10-16   | Patrick Hermann | added task to install ansible collections
|2020-10-08   | Marcel Zapf | fixed some bugs
|2020-08-24   | Patrick Hermann  | fixed task names, fixed os task loop
|2020-08-18   | Marcel Zapf   | initial commit, added tests and readme

Author Information
------------------

```yaml
Andre Ebert (andre.ebert@sva.de); 05/2024

Marcel Zapf (marcel.zapf@sva.de; Stuttgart-Things; 08/2020);

Patrick Hermann (patrick.hermann@sva.de; Stuttgart-Things; 08/2020)
```

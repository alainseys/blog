---
tags:
    - ansible
    - molecule
---
# Ansible Molecule
Molecule is used for testing your ansible roles this is a neat feature.

To install molecule this can be done verry easly by adding the following to your requirements.txt inside your project.

```python
molecule
molecule-docker
pytest-molecule
```

Here you can see i have added also docker , in my setup i test evrything against docker. This allows me to develop much faster.

## Usage
Create e new module or open an existing one update meta/main
```yaml
galaxy_info:
    author: Seys Alain
    description: Install Linux Updates
    company: Seys Consults
    role_name: linuxupdates
    namespace: seysconsults
```
Next run the init command
```molecule init scenario ```

Now update converge.yml in molecule folder
```yaml
# Converge.yml
---
- name: Converge
  hosts: all
  gather_facts: true
  vars:
    ansible_python_interpreter: /usr/bin/python3
  tasks:
    - name: Execute main.yml
      ansible.builtin.import_role:
        name: name-of-your-role
```

Next up edit molecule.yml
```yaml
---
driver:
  name: docker
platforms:
  - name: centos8
    image: geerlingguy/docker-fedora40-ansible:latest
    pre_build_image: true
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    privileged: true
  - name: debian12
    image: geerlingguy/docker-debian12-ansible:latest
    pre_build_image: true
    command: ${MOLECULE_DOCKER_COMMAND:-""}
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:rw
    privileged: true
provisioner:
  name: ansible
verifier:
  name: ansible
lint: |
  set -e
  yamllint .
  ansible-lint .
```
In this example i am using the docker images of [Jef Geerling](https://github.com/geerlingguy) since these images are pretty stable and extendable (if you create your own docker images based on these examples).

Thats it your molecule is ready to be run.

Example usage:
molecule test && molecule converge

### Bonus points
Molecule is exendable with some reporting to html i have generated a bash script so if i run this command this generates a html report in a central location.

```bash
#!/bin/bash
read - p "Enter the report name " injected_name
set_date=$(date +"%d_%m_%y")

pytest --html="/workspaces/awx/Reports-UnitTests/$injected_name"_"$set_date".html
```

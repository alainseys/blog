---
tags:
   - ansible
   - automation
   - vault
---
# Ansible Vault

In this turtorial i will explain in a few easys steps how you can use ansible vault in your projects.

## Prerequisites

You have a working instance of ansible to easy manage the vault i will use vscode to edit the files.

Navigate to your project and enter the following command

```
touch vault.yml
```

### Create secrets

to create secret populate the vaul.yml with some sample data

```
vcenter_username: ansible
vcenter_password: password123!
```

Now our secrets are visible plain text , this is not the wanted results to encrypt the file run the command.
``` ansible-vault encrypt vault.yml ```
This wil prompt you to enter a password choose a secure password and remeber it

when you have succefully encrypted the file wil look something like this 
```
$ANSIBLE_VAULT;1.1;AES256
34633361356538656462333030383838656130623833333130366230386537396637303064653539
6434613663636636383636633739353536383636633363320a396562636634623739656566306639
32356535613136323230616331343639353937633864353666656231303166323365616636326261
3435366634636230360a656166306362373137653863333735663763313139353931376639323738
33313861646436346336396661353735393130386531316137653332333261353235623761303961
37623462646239663565386132633033393162336364333137383964636230353861623733666266
643830366265396564306439336233623033
```

To decrypt the vault back again you can run
``` ansible-vauld decrypt vault.yml ``` this wil ask the decryption password

### Usage in playbook
If you want to use it in a playbook you can use this 

```yaml
- name: Deploy windows vm on vcenter
  hosts: all
  vars_files:
    - ../vault.yml
  tasks:
   - name: Create windwos vm on vcenter
     ansible.builtin.include_role:
       name: vcenter/create-windows-vm
```
Notice the location of your ansible vault this should match the exact location, i am running ansible playbooks using awx so the decryption password is in the credentials.

If you run ansible playbook on the cli you need to pass the password on the command line this should be something like this.

```
ansible-playbook --vault-password-file vault.yml --ask-vault-pass playbook.yml
```

This will prompt you to enter the password you can pass the password in the same way , but this is a security breach.

Reference: [https://docs.ansible.com/ansible/2.8/user_guide/vault.html](https://docs.ansible.com/ansible/2.8/user_guide/vault.html)


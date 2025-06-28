Install the role `linux-system-roles.selinux` using Ansible Galaxy:

```bash
ansible-galaxy role install linux-system-roles.selinux -p roles/
```

Create a playbook named selinux.yml:

```selinux.yml
- name: Selinux config 1
  hosts: all
  become: True
  vars:
    - selinux_state: permissive
  roles:
    - ./roles/linux-system-roles.selinux
```

Execute the playbook to set SELinux to permissive mode:

```bash
ansible-playbook selinux.yml
```

Check the SELinux status on all hosts:

```bash
ansible all -a 'sestatus' -b
```

To set SELinux to enforcing mode create another playbook:

```selinux2.yml
- name: Selinux config 2
  hosts: all
  become: True
  vars:
    - selinux_state: enforcing
  roles:
    - ./roles/linux-system-roles.selinux
```

Execute the second playbook to set SELinux to enforcing mode:

```bash
ansible-playbook selinux2.yml
```

Finally, check the SELinux status again:

```bash
ansible all -a 'sestatus' -b
```

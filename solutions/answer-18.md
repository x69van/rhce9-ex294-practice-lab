Install the role `linux-system-roles.selinux` (via the RHEL system roles package, or Galaxy):

```bash
sudo dnf install -y rhel-system-roles
# or: ansible-galaxy role install linux-system-roles.selinux -p roles/
```

Create a playbook named selinux.yml:

```selinux.yml
- name: Selinux config 1
  hosts: all
  become: true
  vars:
    selinux_state: permissive
  roles:
    - linux-system-roles.selinux
```

Execute the playbook to set SELinux to permissive mode:

```bash
ansible-playbook selinux.yml
```

Check the SELinux status on all hosts:

```bash
ansible all -a 'sestatus' -b
```

Create another copy of the playbook and set SELinux to enforcing:

```bash
cp selinux.yml selinux2.yml
```

```selinux2.yml
- name: Selinux config 2
  hosts: all
  become: true
  vars:
    selinux_state: enforcing
  roles:
    - linux-system-roles.selinux
```

Install ansible-navigator, then execute the second playbook with it:

```bash
sudo pip3 install ansible-navigator
ansible-navigator run selinux2.yml --mode stdout
```

Finally, check the SELinux status again:

```bash
ansible all -a 'sestatus' -b
```

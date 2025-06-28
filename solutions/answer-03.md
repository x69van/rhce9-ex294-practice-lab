Create playbook to install packages on all hosts, with specific conditions for different groups.

```packages.yml
- name: Packages config
  hosts: all
  become: True
  tasks:

    - name: Install php and mariadb
      when: inventory_hostname in groups['dev'] or inventory_hostname in groups['test'] or inventory_hostname in groups['prod']
      loop:
        - php
        - mariadb
      ansible.builtin.dnf:
        name: "{{ item }}"
        state: latest

    - name: Install the 'RPM Development tools' package group
      when: inventory_hostname in groups['dev']
      ansible.builtin.dnf:
        name: '@RPM Development Tools'
        state: latest

    - name: Update all packages 
      when: inventory_hostname in groups['dev']
      ansible.builtin.dnf:
        name: '*'
        state: latest
```

Run the playbook with the command:

```bash
ansible-playbook packages.yml
```

Validate with ad-hoc command:

```bash
ansible dev,test,prod -a 'which mariadb php'
ansible dev -a 'dnf group list installed' -b
```

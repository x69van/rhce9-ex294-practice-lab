Create file called `user_list.yml` with the following content:

```user_list.yml
users:
  - name: adam
    job: developer
    uid: 3000
  - name: gabriel
    job: manager
    uid: 3001
  - name: lucifer
    job: developer
    uid: 3002
```

Create file called `create_user.yml` with the following content:

```create_user.yml
- name: Creating users on dev and test groups
  hosts: dev, test
  become: True
  vars_files:
    - ./vault.yml
    - ./user_list.yml
  tasks:

    - name: ensure group "devops" exists
      ansible.builtin.group:
        name: devops
        state: present

    - name: Adding user with developer description
      loop: "{{ users }}"
      when: item.job == 'developer'
      ansible.builtin.user:
        name: "{{ item.name }}"
        shell: /bin/bash
        uid: "{{ item.uid }}"
        groups: devops
        append: True
        password: "{{ dev_pass | password_hash('sha512') }}"

- name: Creating users on prod group
  hosts: prod
  become: True
  vars_files:
    - ./vault.yml
    - ./user_list.yml
  tasks:

    - name: ensure group "opsmgr" exists
      ansible.builtin.group:
        name: opsmgr
        state: present

    - name: Adding user with manager description
      loop: "{{ users }}"
      when: item.job == 'manager'
      ansible.builtin.user:
        name: "{{ item.name }}"
        shell: /bin/bash
        uid: "{{ item.uid }}"
        groups: opsmgr
        append: True
        password: "{{ mgr_pass | password_hash('sha512') }}"
```

Confirm syntax and run the playbook:

```bash
ansible-playbook --syntax-check create_user.yml --vault-password-file=./password.txt 
```

```bash
ansible-playbook create_user.yml --vault-password-file=./password.txt
```

Validate the users were created:

```bash
ansible prod -a 'id gabriel'
ansible test -a 'id adam lucifer'
ansible dev -a 'id adam lucifer'
```

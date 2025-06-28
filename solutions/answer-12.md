Create a playbook named `issue.yml` with the following content:

```issue.yml
- name: Issue file config
  hosts: all
  become: True
  tasks:

    - name: Replaces /etc/issue on dev group
      when: inventory_hostname in groups['dev']
      ansible.builtin.copy:
        content: 'Development'
        dest: /etc/issue

    - name: Replaces /etc/issue on test group
      when: inventory_hostname in groups['test']
      ansible.builtin.copy:
        content: 'Test'
        dest: /etc/issue

    - name: Replaces /etc/issue on prod group
      when: inventory_hostname in groups['prod']
      ansible.builtin.copy:
        content: 'Production'
        dest: /etc/issue
```

Execute the playbook with the following command:

```bash
ansible-playbook issue.yml
```

Validate the changes by running the following commands:

```bash
ansible dev -a 'cat /etc/issue'
ansible test -a 'cat /etc/issue'
ansible prod -a 'cat /etc/issue'
```

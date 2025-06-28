Create a playbook named `test.yml` that does the following:

```test.yml
- name: Test config
  hosts: test
  become: True
  tasks:

    - name: Ensure group "webtest" exists
      ansible.builtin.group:
        name: webtest
        state: present

    - name: Create a directory /webtest
      ansible.builtin.file:
        path: /webtest
        state: directory
        group: webtest
        mode: '2775'

    - name: Create a directory /var/www/html/webtest
      ansible.builtin.file:
        path: /var/www/html/webtest
        state: directory

    - name: Create a symbolic link
      ansible.builtin.file:
        src: /var/www/html/webtest
        dest: /webtest
        state: link
        force: True

    - name: Creating file  
      ansible.builtin.lineinfile:
        path: /webtest/index.html
        line: Testing.
        create: True
```

Execute the playbook with the command:

```bash
ansible-playbook test.yml
```

Validate with ad-hoc commands:

```bash
ansible test -a 'cat /var/www/html/webtest/index.html'
ansible test -a 'ls -ld /var/www/html/webtest'
```

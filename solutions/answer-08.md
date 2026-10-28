Create a playbook named `test.yml` that does the following:

```test.yml
- name: Test config
  hosts: test
  become: true
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

    - name: Ensure /var/www/html exists
      ansible.builtin.file:
        path: /var/www/html
        state: directory

    - name: Create a symbolic link
      ansible.builtin.file:
        src: /webtest
        dest: /var/www/html/webtest
        state: link

    - name: Creating file
      ansible.builtin.copy:
        content: "Testing.\n"
        dest: /webtest/index.html
```

Execute the playbook with the command:

```bash
ansible-playbook test.yml
```

Validate with ad-hoc commands:

```bash
ansible test -a 'cat /var/www/html/webtest/index.html'
ansible test -a 'ls -ld /webtest /var/www/html/webtest'
```

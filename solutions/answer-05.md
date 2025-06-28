Create role apache in roles/ path

```bash
ansible-galaxy role init apache --init-path=./roles/
```

Create vars file in roles/apache/vars/main.yml

```vars/main.yml
# vars file for apache
packages:
  - httpd
  - firewalld
rules:
  - https
  - http
```

Configure the role in roles/apache/tasks/main.yml

```tasks/main.yml
# tasks file for apache
- name: Install the latest version of Apache and firewalld
  loop: "{{ packages }}"
  ansible.builtin.dnf:
    name: "{{ item }}"
    state: latest

- name: Start service httpd/firewalld and enabled
  loop: "{{ packages }}"
  ansible.builtin.service:
    name: "{{ item }}"
    state: started
    enabled: True

- name: Permanently enable https/http service, also enable it immediately if possible
  loop: "{{ rules }}"
  ansible.posix.firewalld:
    service: "{{ item }}"
    state: enabled
    permanent: True
    immediate: True

- name: Template a file
  ansible.builtin.template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: '0644'
```

Create a template file in roles/apache/templates/index.html.j2

```index.html.j2
Welcome to {{ ansible_facts['fqdn'] }}.cluster.loc on {{ ansible_facts['default_ipv4']['address'] }}
```

Create a playbook to deploy the role in a file named apache-role.yml

```apache-role.yml
- name: Apache role deploy
  hosts: webservers
  become: True
  roles:
    - apache
```

Execute the playbook

```bash
ansible-playbook apache-role.yml
```

Validate the deployment by checking the Apache service status and accessing the web page

```bash
curl vm-node3
curl vm-node4
```

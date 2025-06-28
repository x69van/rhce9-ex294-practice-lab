Create a Jinja2 template file named `hosts.j2`:

```hosts.j2
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       localhost localhost.localdomain localhost6 localhost6.localdomain6
{% for node in groups['all'] %}
{{ hostvars[node]['ansible_facts']['default_ipv4']['address'] }} {{ hostvars[node]['ansible_facts']['fqdn'] }} {{ hostvars[node]['ansible_facts']['hostname'] }}
{% endfor%}
```

Create an Ansible playbook named `gen_hosts.yml`:

```gen_hosts.yml
- name: Hosts config deploy
  hosts: all
  become: True
  tasks:

    - name: Template a file to /etc/myhosts
      when: inventory_hostname in groups['dev']
      ansible.builtin.template:
        src: ./hosts.j2
        dest: /etc/myhosts
```

Execute the playbook with the following command:

```bash
ansible-playbook gen_hosts.yml
```

Validate with ad-hoc command:

```bash
ansible dev -a 'cat /etc/myhosts'
```

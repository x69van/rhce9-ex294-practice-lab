Create a Jinja2 template named `hwreport.j2` that outputs the following information about the host:

```hwreport.j2
INVENTORY_HOSTNAME={{ ansible_facts['hostname'] }}
TOTAL_MEMORY_IN_MB={{ ansible_facts['memtotal_mb'] }}
BIOS_VERSION={{ ansible_facts['bios_version'] }}
```

Create a playbook named `hwreport.yml` with the following content:

```hwreport.yml
- name: Hardware report deploy
  hosts: all
  become: True
  tasks:

    - name: Template a file to /etc/file.conf
      ansible.builtin.template:
        src: ./hwreport.j2
        dest: /root/hwreport.txt
        mode: '0644'
```

Execute the playbook with the command:

```bash
ansible-playbook hwreport.yml
```

Validate with ad-hoc command:

```bash
ansible all -a 'cat /root/hwreport.txt' -b
```

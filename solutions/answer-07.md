Create a file named `squid.yml`:

```
- name: Squid deploy
  hosts: balancers
  become: True
  roles:
    - squid
```

Execute the playbook with the following command:

```bash
ansible-playbook squid.yml
```

Validate with ad-hoc commands:

```bash
ansible balancers -a 'systemctl status squid' -b
ansible balancers -a 'which squid'
```

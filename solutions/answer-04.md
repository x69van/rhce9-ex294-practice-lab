Install the role in roles/ path

```bash
ansible-galaxy role install linux-system-roles.timesync -p roles/
```

Then, create a playbook to configure the NTP service using the role:

```timesync.yaml
- name: Timesync config
  hosts: all
  become: True
  vars:
    timesync_ntp_servers:
      - hostname: 172.25.254.250
        iburst: true
  roles:
    - linux-system-roles.timesync
```

Execute the playbook:

```bash
ansible-playbook timesync.yml
```

Validate with ad-hoc command:

```bash
ansible all -a 'grep 172 /etc/chrony.conf'
```

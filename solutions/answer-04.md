Install the RHEL system roles package:

```bash
sudo dnf install -y rhel-system-roles
```

Then, create a playbook `/home/x69van/ansible/timesync.yml` to configure the NTP service using the role:

```timesync.yml
- name: Timesync config
  hosts: all
  become: true
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

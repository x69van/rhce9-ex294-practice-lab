Create a cron job file named `cron.yml` with the following content:

```cron.yml
- name: Cronjob config
  hosts: all
  become: True
  tasks:

    - name: Add the user 'natasha'
      ansible.builtin.user:
        name: natasha
        shell: /bin/bash

    - name: Cronjob for natasha
      ansible.builtin.cron:
        name: "Cron under natasha user"
        minute: "*/2"
        user: natasha
        job: 'logger "EX294 exam in progress"'
```

Execute the playbook using the following command:

```bash
ansible-playbook cron.yml
```

Validate the cron job by checking the cron entries for the user `natasha`:

```bash
ansible all -a 'crontab -l -u natasha' -b
```

Validate /var/log/messages to ensure the cron job is logging correctly:

```bash
ansible all -a 'tail -n 10 /var/log/messages' -b
```

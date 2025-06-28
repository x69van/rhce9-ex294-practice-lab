Create a playbook named `partition.yml` with the following content:

```partition.yml
- name: Partition config
  hosts: all
  become: True
  tasks:

    - name: Checking details
      block:

        - name: Validate if sdb disk exists
          when: "'sdb' not in ansible_devices"
          ansible.builtin.debug:
            msg: this disk does not exist.

        - name: Create a new ext4 primary partition 1200MiB
          community.general.parted:
            device: /dev/sdb
            number: 1
            state: present
            fs_type: ext4
            part_end: 1200MiB
          when: "'sdb' in ansible_devices"
 
      rescue:
        - name: Cannot create of that size?
          when: "'sdb' in ansible_devices"
          ansible.builtin.debug:
            msg: Could not create partition of that size

        - name: Create a new ext4 primary partition 800MiB
          community.general.parted:
            device: /dev/sdb
            number: 1
            state: present
            fs_type: ext4
            part_end: 800MiB
          when: "'sdb' in ansible_devices"

      always:

        - name: Create a ext4 filesystem on /dev/sdb1
          when: "'sdb' in ansible_devices"
          community.general.filesystem:
            fstype: ext4
            dev: /dev/sdb1

        - name: Mounting ...
          when: inventory_hostname in groups['prod']
          ansible.posix.mount:
            path: /srv
            src: /dev/sdb1
            fstype: ext4
            state: mounted
```

Execute the playbook with the following command:

```bash
ansible-playbook partition.yml
```

To verify the partition creation, you can run the following command:

```bash
ansible all -a 'lsblk /dev/sdb' -b
```

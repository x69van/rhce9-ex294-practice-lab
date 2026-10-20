Install the `community.general` collection to use the `lvol` module for managing LVM logical volumes. You can do this by running the following command:

```bash
ansible-galaxy collection install community.general -p mycollection/
```

Create a playbook file named `lvm.yml` with the following content:

```lvm.yml
- name: LVM config
  hosts: all
  become: true
  tasks:

    - name: Validate details
      block:
        - name: Validate if vg 'research' exists
          when: ansible_lvm.vgs is not defined or 'research' not in ansible_lvm.vgs
          ansible.builtin.debug:
            msg: volume group does not exist

        - name: Create a logical volume of 1200m
          when: ansible_lvm.vgs is defined and 'research' in ansible_lvm.vgs
          community.general.lvol:
            vg: research
            lv: data
            size: 1200m

      rescue:
        - name: Cannot create a lv?
          when: ansible_lvm.vgs is defined and 'research' in ansible_lvm.vgs
          ansible.builtin.debug:
            msg: could not create logical volume of that size

        - name: Create a logical volume of 800m
          when: ansible_lvm.vgs is defined and 'research' in ansible_lvm.vgs
          community.general.lvol:
            vg: research
            lv: data
            size: 800m

      always:

        - name: Create a ext4 filesystem on /dev/research/data
          when: ansible_lvm.vgs is defined and 'research' in ansible_lvm.vgs
          community.general.filesystem:
            fstype: ext4
            dev: /dev/research/data
```

Execute the playbook using the following command:

```bash
ansible-playbook lvm.yml
```

Validate the results by checking the logical volume and filesystem creation:

```bash
ansible all -a 'lsblk' -b
ansible all -a 'lsblk /dev/research/data --fs' -b
```

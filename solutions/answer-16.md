Install the `community.general` collection to use the `lvol` module for managing LVM logical volumes. You can do this by running the following command:

```bash
ansible-galaxy collection install community.general -p mycollection/
```

Create a playbook file named `lvm.yml` with the following content:

```lvm.yml
- name: LVM config
  hosts: all
  become: True
  tasks:

    - name: Validate datails
      block:
        - name: Validate if vgs 'research' exists
          when: "'research' not in ansible_lvm.vgs"
          ansible.builtin.debug:
            msg: volume group does not exist

        - name: Create a logical volume of 1200m
          when: "'research' in ansible_lvm.vgs"
          community.general.lvol:
            vg: research
            lv: data
            size: 1200m

      rescue:
        - name: Cannot create a lv ?
          when: "'research' in ansible_lvm.vgs"
          ansible.builtin.debug:
            msg: could not create logical volume of that size
 
        - name: Create a logical volume of 800m
          when: "'research' in ansible_lvm.vgs"
          community.general.lvol:
            vg: research
            lv: data
            size: 800m

      always:

        - name: Create a ext4 filesystem on /dev/research/data
          when: "'data' in ansible_lvm.lvs"
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

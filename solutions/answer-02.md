Create a playbook to configure the repositories for BaseOS and AppStream.

```yum-repo.yml
- name: Repo config
  hosts: all
  become: true
  tasks:

    - name: Add repository BaseOS
      ansible.builtin.yum_repository:
        name: BaseOS
        baseurl: file:///mnt/BaseOS/
        description: Base OS Repo
        gpgcheck: true
        enabled: false
        gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux-9

    - name: Add repository AppStream
      ansible.builtin.yum_repository:
        name: AppStream
        baseurl: file:///mnt/AppStream/
        description: AppStream Repo
        gpgcheck: true
        enabled: false
        gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-AlmaLinux-9
```

Execute the playbook with the following command:

```bash
ansible-playbook yum-repo.yml
```

Validate with ad-hoc command:

```bash
ansible all -a 'dnf repolist disabled' -b
```

Create a playbook to configure the repositories for BaseOS and AppStream.

```yum-repo.yml
- name: Repo config
  hosts: all
  become: True
  tasks:

    - name: Add repository BaseOS
      ansible.builtin.yum_repository:
        name: BaseOS
        baseurl: file:///mnt/BaseOS/
        description: Base OS Repo
        gpgcheck: True
        enabled: False
        gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
    
    - name: Add repository AppStream
      ansible.builtin.yum_repository:
        name: AppStream
        baseurl: file:///mnt/AppStream/
        description: AppStream Repo
        gpgcheck: True
        enabled: False
        gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```

Execute the playbook with the following command:

```bash
ansible-playbook yum-repo.yml
```

Validate with ad-hoc command:

```bash
ansible all -a 'dnf repolist disabled' -b
```

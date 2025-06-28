Create a `requirements.yml` file to specify the roles you want to install from Ansible Galaxy. The file should include the following roles:

```requirements.yml
- name: zabbix
  src: https://galaxy.ansible.com/download/zabbix-zabbix-1.0.6.tar.gz
- name: security
  src: https://galaxy.ansible.com/download/openafs_contrib-openafs-1.9.0.tar.gz
- name: squid
  src: https://galaxy.ansible.com/download/mafalb-squid-0.2.0.tar.gz
```

Install the roles using the following command:

```bash
ansible-galaxy role install -p roles/ -r requirements.yml
```

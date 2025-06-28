Create inventory file for Ansible with the following content:

```inventory
[dev]
ansible-node-1

[test]
ansible-node-2

[prod]
ansible-node-3
ansible-node-4

[balancers]
ansible-node-5

[webservers:children]
prod
```

Create `ansible.cfg` file with the following content:

```ansible.cfg
[defaults]
inventory=./inventory
roles_path=./roles
collections_path=./mycollection
remote_user=x69van
host_key_checking=False
private_key_file=~/.ssh/RH294-LAB
```

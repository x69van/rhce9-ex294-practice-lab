Install the required packages and prepare the working directory:

```bash
sudo dnf install -y ansible-core
mkdir -p /home/x69van/ansible/{roles,mycollection}
cd /home/x69van/ansible
```

Create inventory file `/home/x69van/ansible/inventory` with the following content:

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

Create `/home/x69van/ansible/ansible.cfg` with the following content:

```ansible.cfg
[defaults]
inventory=./inventory
roles_path=./roles:/usr/share/ansible/roles
collections_path=./mycollection
remote_user=x69van
host_key_checking=false
private_key_file=~/.ssh/RH294-LAB
```

Create password.txt file with the following content:

```bash
echo 'rh294lab' > password.txt
```

Create vault.yml encrypted file using Ansible Vault with the command:

```bash
ansible-vault create vault.yml --vault-password-file=./password.txt
```

With the following content:

```yaml
dev_pass: redhat
mgr_pass: linux
```

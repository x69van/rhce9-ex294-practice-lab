Change the password of an Ansible Vault file using a password file.

```bash
echo 'ansible' > password.txt
```

Rekey the vault file using the following command:

```bash
ansible-vault rekey vault.yml --vault-id @prompt
Vault password (default): 
New Vault password: 
Confirm New Vault password: 
Rekey successful
```

Validate the rekeying by viewing the contents of the vault file using the new password file:

```bash
ansible-vault view vault.yml --vault-password-file=./password.txt 
```

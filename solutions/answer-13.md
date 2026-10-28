Create a temporary file with the new vault password, then rekey using the existing and new password files:

```bash
echo 'ansible' > new_password.txt
ansible-vault rekey vault.yml \
  --vault-password-file=./password.txt \
  --new-vault-password-file=./new_password.txt
mv new_password.txt password.txt
```

Validate the rekeying by viewing the contents of the vault file using the new password file:

```bash
ansible-vault view vault.yml --vault-password-file=./password.txt
```


```bash
# Create a password for vault
sesha@LAPTOP-QMBUJPPJ:~$ openssl rand -base64 2048 > vault.pass

# Encrypt the file
sesha@LAPTOP-QMBUJPPJ:~$ ansible-vault create vault.yaml --vault-password-file vault.pass

# Open the encrypted file
sesha@LAPTOP-QMBUJPPJ:~$ cat vault.yaml
$ANSIBLE_VAULT;1.1;AES256
316134316

# Decrypt the file
sesha@LAPTOP-QMBUJPPJ:~$ ansible-vault decrypt vault.yaml --vault-password-file vault.pass
Decryption successful
sesha@LAPTOP-QMBUJPPJ:~$ cat vault.yaml
ec2_access: fdfd
ec2_secrete: dfdl

# Add a new password to it
sesha@LAPTOP-QMBUJPPJ:~$ ansible-vault decrypt vault.yaml --vault-password-file vault.pass
Decryption successful
sesha@LAPTOP-QMBUJPPJ:~$ cat vault.yaml
ec2_access: fdfd
ec2_secrete: dfdl
api_token: ghi

```


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

# View the file without decrypting
sesha@LAPTOP-QMBUJPPJ:~$ ansible-vault view vault.yaml --vault-password-file vault.pass
ec2_access: fdfd
ec2_secrete: dfdl
api_token: ghi

# Encrypt a single value instead of encrypting the entire file
ansible-vault encrypt_string foobar --vault-password-file vault.pass
```

## Summarize

The video explains **Ansible Vault**, which is used to securely store sensitive information inside Ansible projects.

Main topics covered:

* Why security is important in DevOps

  * Sensitive data like passwords, AWS keys, API tokens should never be stored in plain text.
  * Uploading playbooks with secrets to Git can expose credentials.

* What Ansible Vault does

  * Encrypts files or variables containing secrets.
  * Only users with the vault password can view or modify the content.

* Real-world example

  * AWS EC2 playbook requires:

    * AWS Access Key
    * AWS Secret Key
  * Instead of storing them directly in playbooks, they are stored in encrypted Vault files.

Important commands explained:

Create encrypted file:

```bash id="y8ll7i"
ansible-vault create aws_credentials.yaml
```

Encrypt existing file:

```bash id="gcw49o"
ansible-vault encrypt file.yaml
```

Decrypt file:

```bash id="orqv1h"
ansible-vault decrypt aws_credentials.yaml
```

Edit encrypted file:

```bash id="0wkg83"
ansible-vault edit aws_credentials.yaml
```

View encrypted file:

```bash id="z4y9b0"
ansible-vault view aws_credentials.yaml
```

Encrypt only one variable/string:

```bash id="pdx9z1"
ansible-vault encrypt_string foobar --vault-password-file vault.pass
```

Vault password handling:

* You can:

  * enter password manually
  * or use:

```bash id="f1zwqv"
--vault-password-file vault.pass
```

Best practices discussed:

* Use strong passwords.
* Do not store vault password files openly.
* Store vault passwords in secret managers like:

  * AWS Secrets Manager
  * AWS Systems Manager Parameter Store
  * Azure Key Vault

Environment-based password strategy:

* Use different vault passwords for:

  * Dev
  * Staging
  * Production
* This limits impact if one environment password gets compromised.

Other key points:

* Vault works similar to a bank locker:

  * password required to open/edit.
* Secrets are usually accessed in playbooks through variables and Jinja2 templates.
* No extra installation is required because Ansible Vault comes built into Ansible.

Final assignment suggested in the video:

* Practice AWS EC2 instance creation playbook using:

  * encrypted AWS credentials
  * Ansible Vault
  * boto3
  * AWS collection
  * Jinja2 variables.

---

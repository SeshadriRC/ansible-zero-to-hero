# Setup EC2 Collection and Authentication

## Collection

- [definition](https://github.com/SeshadriRC/ansible-zero-to-hero/blob/main/Day-03/02-ansible-playbook-components.md#collections)

- [Collections-ansible-galaxy]
  
<img width="1918" height="806" alt="image" src="https://github.com/user-attachments/assets/649f7ef5-421c-4410-b2c1-323e8ba9fb67" />


## Install boto3

```
pip install boto3
```

## Install AWS Collection

```
ansible-galaxy collection install amazon.aws
```

## Setup Vault 

1. Create a password for vault

```
openssl rand -base64 2048 > vault.pass
```

2. Add your AWS credentials using the below vault command

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```






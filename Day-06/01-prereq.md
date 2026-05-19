<img width="768" height="141" alt="image" src="https://github.com/user-attachments/assets/9900ab8d-2103-404a-8587-bba29c85d450" /># Setup EC2 Collection and Authentication

## Collection

- [definition](https://github.com/SeshadriRC/ansible-zero-to-hero/blob/main/Day-03/02-ansible-playbook-components.md#collections)

- [Collections-ansible-galaxy](https://galaxy.ansible.com/ui/repo/published/amazon/aws/)
  
<img width="1918" height="806" alt="image" src="https://github.com/user-attachments/assets/649f7ef5-421c-4410-b2c1-323e8ba9fb67" />


## Install boto3

<img width="1910" height="374" alt="image" src="https://github.com/user-attachments/assets/f6b55996-7659-4fd8-a350-8a3daf3b868c" />


```
pip install boto3
```



## Install AWS Collection

```
ansible-galaxy collection install amazon.aws
```


## Create a role

```
ansible-galaxxy role init ec2
```

- In the folder of `tasks` , copy the `yaml`

```yaml
- name: start an instance with a public IP address
    amazon.aws.ec2_instance:
      name: "ansible-instance"
      # key_name: "prod-ssh-key"
      # vpc_subnet_id: subnet-013744e41e8088axx
      instance_type: t2.micro
      security_group: default
      region: us-east-1
      aws_access_key: "{{ec2_access_key}}"  # From vault as defined, this is nothing but a built in variable
      aws_secret_key: "{{ec2_secret_key}}"  # From vault as defined      
      network:
        assign_public_ip: true
      image_id: ami-04b70fa74e45c3917
      tags:
        Environment: Testing
```

<img width="995" height="352" alt="image" src="https://github.com/user-attachments/assets/b8f99f90-49d0-47c0-81b0-229b765b32cb" />

<img width="1308" height="481" alt="image" src="https://github.com/user-attachments/assets/05a8cefd-759f-4f25-a480-c974bb2a1e86" />


## Setup Vault 

1. Create a password for vault

```
openssl rand -base64 2048 > vault.pass
```

2. Add your AWS credentials using the below vault command.

- Generate a Access key and Secret Access key and store it in below file

```
# To create a file
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass

# For editing
ansible-vault edit group_vars/all/pass.yml --vault-password-file vault.pass
```

<img width="768" height="141" alt="image" src="https://github.com/user-attachments/assets/8413d1bb-f1ba-4a44-a582-f47427ff2106" />


<img width="1384" height="530" alt="image" src="https://github.com/user-attachments/assets/fe452255-a988-4475-8a64-d1a093e67eb4" />


3. Create a EC2 instance

```
ansible-playbook create_ec2.yaml --vault-password-file vault.pass
```




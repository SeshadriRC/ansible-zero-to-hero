<img width="768" height="141" alt="image" src="https://github.com/user-attachments/assets/9900ab8d-2103-404a-8587-bba29c85d450" /># Setup EC2 Collection and Authentication

## Collections

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
ansible-playbook ec2-create.yml --vault-password-file vault.pass
```


---
# Variables

- In Ansible, a variable is a placeholder used to store values that can be reused dynamically in playbooks.
- [Variables Precedence](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable) -> Total in 22 places we will be using variables


- In this video explained below
    - Precedence for vars `main.yaml` is more than defaults `main.yaml`
    - extra vars
    - group vars --> we will be using this in inventory

- Precedence for vars `main.yaml` is more than defaults `main.yaml`
<img width="1127" height="838" alt="image" src="https://github.com/user-attachments/assets/862f4f8f-792b-4025-9021-b149e3adda6d" />

--- 

# Group vars

## Folder Structure

```text id="4o3rkg"
ansible-project/
│
├── inventory
├── playbook.yml
└── group_vars/
    ├── webservers.yml
    └── dbservers.yml
```


## inventory

```ini id="6x6d6v"
[webservers]
server1 ansible_host=192.168.1.10
server2 ansible_host=192.168.1.11

[dbservers]
db1 ansible_host=192.168.1.20
```

This creates:

* `webservers` group
* `dbservers` group

---

### group_vars/webservers.yml

```yaml id="q8jv1l"
package_name: nginx
port: 80
```

Variables for all web servers.

---

## group_vars/dbservers.yml

```yaml id="l07m7z"
package_name: mysql-server
port: 3306
```

Variables for all DB servers.

---

## playbook.yml

```yaml id="2jlwmj"
- hosts: webservers
  become: yes

  tasks:
    - name: Install package
      yum:
        name: "{{ package_name }}"
        state: present

    - name: Print port
      debug:
        msg: "Application runs on port {{ port }}"
```

---

## What Happens

For `webservers`:

* `package_name = nginx`
* `port = 80`

For `dbservers`:

* `package_name = mysql-server`
* `port = 3306`

Ansible automatically loads matching group variables based on inventory group name.

---

## Simple Understanding

```text id="xyplko"
Inventory → Defines server groups
group_vars → Defines variables for those groups
```

---

# extra vars

Here is a simple example of using `--extra-vars` in Ansible with EC2 instance type.

---

## playbook.yml

```yaml id="n8g3m0"
- hosts: localhost
  connection: local

  tasks:
    - name: Print EC2 instance type
      debug:
        msg: "Creating EC2 with instance type {{ instance_type }}"
```

---

## Run Playbook with Extra Vars

```bash id="98evw0"
ansible-playbook playbook.yml --extra-vars "instance_type=t3.medium"
```

---

## Output

```text id="q64n96"
Creating EC2 with instance type t3.medium
```

---

## Why Extra Vars Are Used

Instead of hardcoding:

```yaml id="mjlwm4"
instance_type: t3.medium
```

you pass values dynamically during execution.

Example:

```bash id="6dkyl4"
--extra-vars "instance_type=t2.micro"
```

or

```bash id="e7l7yj"
--extra-vars "instance_type=t3.large"
```

Same playbook, different values.

<img width="1919" height="516" alt="image" src="https://github.com/user-attachments/assets/2b0dd891-dae8-4246-880e-ddb9cbdcb906" />


---

### To create a new role, use below command

```bash
ansible-galaxy role init <role-name>

(ansible314) root@LAPTOP-QMBUJPPJ:~# ansible-galaxy role init test
- Role test was created successfully

(ansible314) root@LAPTOP-QMBUJPPJ:~# ls
Dockerfile  ansible314  aws-keys  awscli  dynamic.py  inventory.ini  test

(ansible314) root@LAPTOP-QMBUJPPJ:~# cd test/

(ansible314) root@LAPTOP-QMBUJPPJ:~/test# ls
README.md  defaults  files  handlers  meta  tasks  templates  tests  vars
```

### Convert a below playbook to ansible role

[playbook-link](https://github.com/SeshadriRC/ansible-zero-to-hero/tree/main/Day-03/03-first-playbook)

- indendation shortuct ( esc --> gg --> =G )

```yaml
- hosts: all
  become: true        # It indicates, play should run as a root user.
  tasks:
    - name: Install apache httpd
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: yes
    - name: Copy file with owner and permissions
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html
        owner: root
        group: root
        mode: '0644'
```

```bash
(ansible314) root@LAPTOP-QMBUJPPJ:~# ansible-galaxy role init httpd
- Role httpd was created successfully

(ansible314) root@LAPTOP-QMBUJPPJ:~# ls
Dockerfile  ansible314  aws-keys  awscli  dynamic.py  httpd  inventory.ini  test

(ansible314) root@LAPTOP-QMBUJPPJ:~# cd httpd/

(ansible314) root@LAPTOP-QMBUJPPJ:~/httpd# ls
README.md  defaults  files  handlers  meta  tasks  templates  tests  vars
```

```
# Edit the main playbook like below
```

```yaml
- hosts: all
  become: true
  roles:
   - httpd
```
<img width="1163" height="270" alt="image" src="https://github.com/user-attachments/assets/877f312a-dff9-4d25-acf0-210596cbc9e8" />


```
# Copied the task section of the playbook, make sure you modified index.html file location
```
<img width="974" height="409" alt="image" src="https://github.com/user-attachments/assets/9ea55cf7-28be-4617-9dba-845fa99cd251" />

```yaml
- name: Install apache httpd
  ansible.builtin.apt:
    name: apache2
    state: present
    update_cache: yes

- name: Copy file with owner and permissions
  ansible.builtin.copy:
    src: files/index.html
    dest: /var/www/html
    owner: root
    group: root
    mode: '0644'
```

```
# Copy the index.html file to the files directory
```

<img width="809" height="129" alt="image" src="https://github.com/user-attachments/assets/055ee273-3188-4da1-ab1f-466720192b78" />


```bash
# Run the ansible playbook

ansible-playbook -i dynamic.py playbook.yml
```

<img width="1919" height="640" alt="image" src="https://github.com/user-attachments/assets/e9806894-5517-46f2-822c-7249fd5c544a" />

- We can see file is copied and httpd is running.

<img width="1100" height="368" alt="image" src="https://github.com/user-attachments/assets/de29e168-34c2-44b3-9f9a-627fd55dd6be" />

- Able to access the application on both the EC2's

<img width="1449" height="467" alt="image" src="https://github.com/user-attachments/assets/a690ff85-6830-4c6e-a017-947163eb1aa5" />
<img width="1718" height="464" alt="image" src="https://github.com/user-attachments/assets/f9d9136a-ae8f-4aaf-8641-111f184342fb" />


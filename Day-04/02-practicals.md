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

# Copied the task section of the playbook
```

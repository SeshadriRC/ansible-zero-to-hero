- To create a new role, use below command

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

<img width="1008" height="512" alt="image" src="https://github.com/user-attachments/assets/a8a48218-5ae1-4f08-b44b-15515755394c" />

- [docker-role-galaxy](https://galaxy.ansible.com/ui/standalone/roles/bsmeding/docker/)

- This role is used to install and configure docker in different environments. We can also open the user github repo to see the source code.

<img width="1919" height="587" alt="image" src="https://github.com/user-attachments/assets/96340fd7-ed35-4421-b714-474fd86d30fe" />

<img width="1918" height="811" alt="image" src="https://github.com/user-attachments/assets/29f5a227-f493-4e13-9986-a0619dea037c" />


### Practicals

- Roles in my namespace

<img width="1898" height="790" alt="image" src="https://github.com/user-attachments/assets/dc370bbf-3b91-4184-91af-77d902a6ff8b" />


- Install the role, copy the command from the galaxy

<img width="1916" height="607" alt="image" src="https://github.com/user-attachments/assets/5426e6d5-c1b9-4227-8392-48db7f8d6de0" />

```bash
ansible-galaxy role install bsmeding.docker
```

<img width="1229" height="287" alt="image" src="https://github.com/user-attachments/assets/eeb3024a-071d-496b-97d0-73403bf5ac75" />

<img width="680" height="160" alt="image" src="https://github.com/user-attachments/assets/4d9ffa04-dbab-47d8-b088-8bb81411ed0a" />

- Write the main yaml `docker-playbook.yml` to call the role

```yaml
- hosts: all
  become: true
  roles:
    - bsmeding.docker
```

<img width="1432" height="246" alt="image" src="https://github.com/user-attachments/assets/a675da13-d1a2-4579-83d5-7386ebfc9db1" />

- I created debian instance, so that docker will install on both different OS.

<img width="1919" height="425" alt="image" src="https://github.com/user-attachments/assets/56018797-b818-406a-b1ce-df099053cd66" />

- Its failing because for debian we need to login with differnt user, but in the `dynamic.py` we provided only `ubuntu` user.
- So it got installed only on `ubuntu` instance. Run the below command and check

```bash
ansible all -i dynamic.py -m shell -a "docker"
```

<img width="1919" height="527" alt="image" src="https://github.com/user-attachments/assets/bb1750f3-275a-4a64-bb61-fa0bb3fa232e" />

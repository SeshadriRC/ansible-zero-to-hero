[galaxy-link](https://galaxy.ansible.com/ui/)

# Push your Ansible roles to Ansible Galaxy

1. Make sure your role is structured correctly. The basic structure should look like this:

```
my_role/
├── defaults/
│   └── main.yml
├── files/
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
├── tests/
│   ├── inventory
│   └── test.yml
└── vars/
    └── main.yml
```

2. Make sure ansible-galaxy CLI exists

```
ansible-galaxy --version
```

3. Push Your Role to GitHub

```
cd <role-name>
git init
git remote add origin <https://github.com/your_github_username/my_role.git>
git add .
git commit -m "Initial commit"
git push -u origin main
```

4. Import the Role to Ansible Galaxy

```
ansible-galaxy role import <your_github_username> <role-name>
```

### Practicals

- Create a dummy repository in github
- [dummy-role-repo-link](https://github.com/SeshadriRC/dummy-role)
- Now navigate inside to the `httpd` role and run command `git init`
  <img width="1178" height="421" alt="image" src="https://github.com/user-attachments/assets/2b24b031-f730-4590-a133-6952eb040491" />

- Add the remote repo
<img width="1446" height="494" alt="image" src="https://github.com/user-attachments/assets/b5af2cd4-ec66-49f4-89b4-42a15017f0ee" />

```
git remote add origin https://github.com/SeshadriRC/dummy-role.git
```
<img width="1217" height="178" alt="image" src="https://github.com/user-attachments/assets/1ca3b958-40f7-4af8-9beb-ec962ddf860d" />

- commit and push it
<img width="1044" height="450" alt="image" src="https://github.com/user-attachments/assets/58272e09-71d9-4e46-8a9f-928bcc4346c6" />

```bash
git add .
git commit -m "ansible-role"
git push
```

- Repo got updated

<img width="1344" height="825" alt="image" src="https://github.com/user-attachments/assets/fd113c30-09ad-456a-a4a9-a6859c4b26b7" />


- Push the role to the ansible galaxy, before that take the API token

<img width="1411" height="487" alt="image" src="https://github.com/user-attachments/assets/c42f90af-1b66-4cba-9b88-f203d42e1bb9" />


```bash
ansible-galaxy import <your_github_username> <role-repo>  --token <API_TOKEN>

ansible-galaxy import SeshadriRC dummy-role --token 69e1ffd14
```

- Role got imported

<img width="1891" height="757" alt="image" src="https://github.com/user-attachments/assets/322f6510-551f-4343-ac95-ab23bfed4e12" />

<img width="1919" height="707" alt="image" src="https://github.com/user-attachments/assets/214ceb62-689e-4061-b7bc-cf9fca022f2e" />

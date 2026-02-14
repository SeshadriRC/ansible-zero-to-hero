
## unarchive

```yaml
- name: Extract archive

  hosts: all
  become: yes
  tasks:
    - name: Extract the archive and set the owner/permissions
      unarchive:
        src: /usr/src/data/nautilus.zip
        dest: /opt/data/
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: "0777"
```

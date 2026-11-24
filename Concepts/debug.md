In Ansible, the `debug`**&#x20;module** is used to print messages, variable values, and command outputs during playbook execution.

It is mainly used for troubleshooting and verifying what is happening inside a playbook.

### **1. Print a simple message**

```
- name: Display a message
  hosts: all
  tasks:
    - name: Print message
      debug:
        msg: "Ansible playbook execution started"
```

Output:

```
TASK [Print message]
ok: [server1] => {
    "msg": "Ansible playbook execution started"
}
```

### **2. Print a variable value**

```
- name: Check application version
  hosts: all
  vars:
    app_version: "17.0"

  tasks:
    - name: Display application version
      debug:
        msg: "Application version is {{ app_version }}"
```

Output:

```
Application version is 17.0
```

### **3. Print registered command output**

This is a very common real-time use case.

```
- name: Check disk usage
  hosts: all
  tasks:

    - name: Execute df command
      shell: df -h /
      register: disk_output

    - name: Display disk usage
      debug:
        var: disk_output.stdout
```

Here:

- `shell` executes the Linux command.
- `register` stores the command result in a variable.
- `debug` displays the stored output.
- `stdout` displays only the standard output.

### **4. Difference between&#x20;**`msg`**&#x20;and&#x20;**`var`

| Parameter | Purpose                                   |
| --------- | ----------------------------------------- |
| `msg`     | Prints a custom message or variable value |
| `var`     | Prints the complete value of a variable   |

Example:

```
- name: Display variable
  debug:
    msg: "Server IP is {{ server_ip }}"

- name: Display complete variable
  debug:
    var: server_ip
```

### **5. Interview scenario**

**Question:** Your Ansible playbook executes a shell command, but you are not sure whether it is returning the expected output. How would you troubleshoot?

**Answer:**

I would use the `register` keyword to capture the command output and the `debug` module to print the registered variable.

```
- name: Check service status
  hosts: all
  tasks:

    - name: Check nginx status
      shell: systemctl is-active nginx
      register: service_status
      changed_when: false

    - name: Display service status
      debug:
        var: service_status.stdout
```

This helps me verify whether the service is running on the target servers.

**Interview tip:** Remember these three together:

- `register` → Capture command output.
- `debug` → Display captured output.
- `when` → Execute tasks conditionally based on a variable or result.


---

```
# Playbook to check whether Nginx service is running
- name: Check service status
  hosts: all

  tasks:

    # Execute the command to check Nginx service status
    - name: Check nginx status
      shell: systemctl is-active nginx

      # Store the command output in the service_status variable
      register: service_status

      # Prevent Ansible from marking this task as changed
      changed_when: false

    # Display the captured service status
    - name: Display service status
      debug:
        # Print only the standard output (stdout) of the command
        var: service_status.stdout
```

**Expected output if Nginx is running:**

```
TASK [Display service status]
ok: [server1] => {
    "service_status.stdout": "active"
}
```

**Important:** `changed_when: false` ensures that Ansible reports the status-check task as `ok` instead of `changed`, because checking service status does not modify anything.

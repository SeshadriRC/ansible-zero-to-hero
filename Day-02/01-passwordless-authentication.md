# How to setup Passwordless Authentication

## EC2 Instances

- Provision two EC2 instances

<img width="1919" height="333" alt="image" src="https://github.com/user-attachments/assets/554a58a0-8abb-4256-ab15-1ed52466e1dc" />


### Using Public Key

```
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>


ssh-keygen
press enter .....

ssh-copy-id -f "-o IdentityFile /mnt/e/WSL/AWS/EC2/first-ec2.pem" ubuntu@15.206.212.162

ssh ubuntu@15.206.212.162
```

- ssh-copy-id: This is the command used to copy your public key to a remote machine.
- -f: This flag forces the copying of keys, which can be useful if you have keys already set up and want to overwrite them.
- "-o IdentityFile <PATH TO PEM FILE>": This option specifies the identity file (private key) to use for the connection. The -o flag passes this option to the underlying ssh command.
- ubuntu@<INSTANCE-IP>: This is the username (ubuntu) and the IP address of the remote server you want to access.

- So after running above command, we can see that public key of  `ansible control node` is copied to the `managed-node01`.

### Using Password 

- Go to the file `/etc/ssh/sshd_config.d/60-cloudimg-settings.conf`
- Update `PasswordAuthentication yes`
- Restart SSH -> `sudo systemctl restart ssh`


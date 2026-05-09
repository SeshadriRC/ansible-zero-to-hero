**for different OS**

```python

#!/usr/bin/env python3

import json
import boto3


def get_aws_ec2_inventory():

    ec2 = boto3.client('ec2')

    instances = ec2.describe_instances()

    inventory = {
        'all': {
            'hosts': []
        },
        '_meta': {
            'hostvars': {}
        }
    }

    for reservation in instances['Reservations']:

        for instance in reservation['Instances']:

            # Skip stopped instances
            if instance['State']['Name'] != 'running':
                continue

            # Skip instances without public IP
            if 'PublicIpAddress' not in instance:
                continue

            public_ip = instance['PublicIpAddress']

            # Get AMI details
            image_id = instance['ImageId']

            image = ec2.describe_images(
                ImageIds=[image_id]
            )['Images'][0]

            ami_name = image.get('Name', '').lower()

            # Detect SSH username based on AMI
            if 'ubuntu' in ami_name:
                ansible_user = 'ubuntu'

            elif 'debian' in ami_name:
                ansible_user = 'admin'

            elif 'amzn' in ami_name or 'amazon' in ami_name:
                ansible_user = 'ec2-user'

            elif 'centos' in ami_name:
                ansible_user = 'centos'

            elif 'rhel' in ami_name or 'redhat' in ami_name:
                ansible_user = 'ec2-user'

            else:
                ansible_user = 'ubuntu'

            # Add host
            inventory['all']['hosts'].append(public_ip)

            # Host variables
            inventory['_meta']['hostvars'][public_ip] = {
                'ansible_host': public_ip,
                'ansible_user': ansible_user,
                'ansible_ssh_private_key_file': '/mnt/e/WSL/AWS/EC2/first-ec2.pem',
                'ansible_ssh_common_args': '-o StrictHostKeyChecking=no'
            }

    print(json.dumps(inventory, indent=2))


if __name__ == '__main__':
    get_aws_ec2_inventory()

```

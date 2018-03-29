# AWS EC2 Ansible Docker example
Create and connect to a AWS EC2 instance made simple

## Prerequisites
* Docker installed
* AWS user with these permissions:
  * `AmazonEC2FullAccess`
  * `SecurityAudit`
* Configurations can be made in the `inventory` file.

## Create EC2 instance

Please add your AWS user credentials to the `inventory` file:
```
ec2_access_key="YOUR_AWS_ACCESS_KEY_ID"
ec2_secret_key="YOUR_AWS_SECRET_ACCESS_KEY"
```

And execute following statement:
```
docker run -it --rm -v $(pwd):/ansible \
ryanwalls/ansible-aws:v2.3.1.0-1 sh -c "ansible-playbook -i ./inventory ec2-basic.yml"
```

## Multiple instances per user
* copy the `id_rsa_your_name.pub` key into the `deploy_keys` directory.
* add role execution to `ec2-basic.yml` like this: 
  ```
  - { role: ec2_basic, key_name: 'id_rsa_joe', tags: basic}
  - { role: ec2_basic, key_name: 'id_rsa_peter', tags: basic}
  - { role: ec2_basic, key_name: 'id_rsa_julia', tags: basic}
  ```

## SSH connection
look up the public ip in the `inventory` file underneath `[playground]` block and pass it to:

`ssh -i ./deploy_keys/<key_name> <ansible_ssh_user>@<PUBLIC_IP>`


## Delete EC2 instance
Uncomment the `FOR DELETION ONLY` block in the `inventory` file and enter the correct ec2 instance id

## Known issues
* `IdempotentInstanceTerminated: The client token you have provided is associated with a terminated instance. Please use a different client token`: Please change the `key_name` variable in `inventory` to create a new instance(s)
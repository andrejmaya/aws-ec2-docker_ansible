# AWS EC2 Ansible Docker example
Create and connect to a AWS EC2 instance made simple

## Prerequisites
* Docker installed
* AWS user with these permissions:
  * `AmazonEC2FullAccess`
  * `SecurityAudit`
* Additional configurations can be made in the `group_vars/all` file

## Create EC2 instance

Export AWS user credentials like this:
  * `export AWS_ACCESS_KEY_ID=...`
  * `export AWS_SECRET_ACCESS_KEY=...`

And execute following statement:
```
docker run -it --rm --name ec2-deployment \
-e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
-v $(pwd):/ec2 ansible/ansible:centos7 cd ec2 &&  ansible-playbook -i ./hosts ec2-basic.yml
```
## SSH connection
look up the public ip in the `hosts` file underneath `[webservers]` block and pass it to:

`ssh -i ec2.id_rsa ubuntu@<PUBLIC_IP>`


## Delete EC2 instance
Uncomment the `FOR DELETION ONLY` block in the `group_vars/all` file and enter the correct ec2 instance id
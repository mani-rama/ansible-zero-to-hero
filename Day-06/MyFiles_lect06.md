# Varibles & Preferences

# this is for my reference of day 06

now to create the resoucer in aws, ansible should talk to api of aws
we use the aws module for connect with api
the module is installed in localhost/control node where is ansible installed,

```
https://docs.ansible.com/ansible/latest/collections/amazon/aws/index.html
```

### collection
if we want to use the aws collection in ansible, first we need to install the aws collection for that we can refer docs or go to galaxy.ansible.com
### To install the aws collection
```
ansible-galaxy collection install amazon.aws
```
in galaxy go to collection>namespace>search aws > install tab

to talk to aws api ansible need python and boto3 
### install boto3
```
pip install boto3
```
Now, to create ec2 instance we need playbook and to get organized sample playbook run below command
```
ansible-galaxy role init ec2
```
ec2 - is directory name which is going to create after the command execution
above command will create 8 file 
1. defaults  # defaults variable we can put here
2. files  # any file we need to copy src to dest we can put here
3. handlers  # after the main task any tast we need to perform that task put here
4. meta  # meta data of playbook
5. tasks  # main tasks playbook file 
6. templates  # any template we are using
7. tests  # test command playbook
8. vars  # varible playbook
once it is create we can write the playbook as

```
- hosts: localhost
  connection: localhost
  roles:
    - ec2
```

inside task > main.yml file
```
- name: start an instance with a public IP address
  amazon.aws.ec2_instance:
    name: "public-compute-instance"
    #key_name: "prod-ssh-key"
    #vpc_subnet_id: subnet-5ca1ab1e
    instance_type: c5.large
    security_group: default
    region: us-east-1
    aws_access_key: "{{ ec2_access_key }}"  # from vault
    aws_secret_key: "{{ ec2_secret_key }}"   # from vault
    network_interfaces:
      - assign_public_ip: true
    image_id: ami-123456
    tags:
      Environment: Testing
```

to talk to api we need access key and secret key from aws 
<br>
if we put access and secret key inside the playbook then secure issue will get so overcome ansible has vault system.

setup ansible-vault
this is given in pre-requisite docs

to run the playbook
```
ansible-playbook -i <any inverntory> ec2.yml --vault-pawwsord-file vault.pass
```
#inverntory file is not required as we are create the resource
we need to pass the vault password with this command 

## variables
Variable has precedence 

https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#ansible-variable-precedence

with this above link we can get the variable and precedence











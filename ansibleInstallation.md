1. CREATE 3-5 SERVERS FOR ANSIBLE TESTING
2. Login to Root user

3. #SET HOSTNAME FOR EASIER IDENTIFICATION
hostnamectl set-hostname ansible
hostnamectl set-hostname dev
bash

#NOW NEED TO SET PASSWORD FOR ROOT IN ALL SERVERS
passwd root   -- then enter password

#
vim /etc/ssh/sshd_config
modify line 38 by removing # and line 63 by replacing no with yes
systemctl restart sshd   #restart sshd service

#NOW WE HAVE TO INSTALL ANSIBLE IN MAIN ANSIBLE SERVER
amazon-linux-extras install ansible2 -y   #this is for amazonlinux

#now installing python dependencies
yum install python python-pip -y
yum install python python-pip python-dlevel

vim /etc/ansible/hosts
enter text after 12 line: 
[test]
private-ip-address of you worker node
[dev]
private-ip

#NOW WE WILL GENERATE SSH RSA KEY: only in Main Ansible Server
ssh-keygen -t rsa  #4 times enter

#WE NEED TO COPY THE KEY ID TO OTHER WORKER NODES
ssh-copy-id root@privateip   #(eg: root@172.31.46.252)
after that enter password of you root user which you have already set

ssh private-ip


#NOW YOU CAN INSTALL THE SOFTWARES FROM MAIN ANSIBLE TO ALL OTHER WORKER NODES:
#ADHOC SIMPLE COMMANDS
ansible all -a "yum install git -y"    #all=all worker nodes, replace all with dev=for dev only worker , -a = arguments
ansible all -a "yum install httpd -y"
ansible dev -a "touch file1"
ansible test -a "touch file2"
ansible all --list-hosts   #to list all the worker nodes/hosts

Goto diveintoansible-lab
Run docker-compose up

[control]
ubuntu-c ansible_connection=local

[centos]
centos1 ansible_port=2222
centos[2:3]

[centos:vars]
ansible_user=root

[ubuntu]
ubuntu[1:3]

[ubuntu:vars]
ansible_become=true
ansible_become_pass=password

[linux:children]
centos
ubuntu

SSH
cat ~/.ssh/known_hosts
ssh-keygen -H -F ubuntu1

Generate public and private key
ssh-keygen

copy public key onto remote host authorized key file..
ssh-copy-id ansible@ubuntu1

on ubuntu
sudo apt update
sudo apt install sshpass

for user in ansible root; do for os in ubuntu centos; do for instance in 1 2 3; do sshpass -f password.txt ssh-copy-id -o StrictHostKeyChecking=no ${user}@${os}${instance}; done; done; done

ansible -i,ubuntu1,ubuntu2,ubuntu3,centos1,centos2,centos3 all -m ping

ansible --version

/etc/ansible/ansible.cfg
/home/ansible/.ansible.cfg - more priority
/home/ansible/testdir/ansible.cfg - current directory
config file = /home/ansible/this_is_example_ansible.cfg - ANSIBLE_CONFIG env - highest priority

export ANSIBLE_CONFIG=/home/ansible/this_is_example_ansible.cfg

change to root user
su -

From current directory, touch ansible.cfg
config file = /home/ansible/testdir/ansible.cfg

unset ANSIBLE_CONFIG - remove env variable

ANSIBLE_HOST_KEY_CHECKING=False ansible all -m ping

cat hosts
ansible all -m ping
ansible centos -m ping
ansible ubuntu -m ping
ansible '*' -m ping
ansible all -m ping -o
ansible centos --list-hosts
ansible ubuntu --list-hosts
ansible all --list-hosts
ansible centos1 --list-hosts
ansible centos1 -m ping -o
ansible ~.*3 -m ping -o

id
ansible all -m command -a 'id' -o
cat hosts
ansible all -i hosts.yaml --list-hosts

ansible --help | more

ansible linux -m ping -e 'ansible_port=2222' -o

ansible all -m file -a 'path=/tmp/test state=touch'

ssh centos2 ls -ali /tmp/test

ansible all -m file -a 'path=/tmp/test state=file mode=600'

ansible all -m copy -a 'src=/tmp/x dest=/tmp/x'
ansible all -m copy -a 'remote_src=yes src=/tmp/x dest=/tmp/y'

ansible all -a 'hostname' -o

ansible all -a 'rm /tmp/test_comman_module removes=/tmp/test_comman_module'

ansible all -m file -a 'path=/tmp/test_module.txt state=touch mode=600' -o

ansible centos1 -m setup -a 'gather_subset=network' | more
ansible centos1 -m setup -a 'gather_subset=network' | wc -l
ansible centos1 -m setup -a 'gather_subset=!all,!min,network' | wc -l
ansible centos1 -m setup -a 'gather_subset=!all,!min,network' | more

ansible centos1 -m setup -a 'filter=ansible_mem*'

ansible-playbook facts_playbook.yaml -l ubuntu-c





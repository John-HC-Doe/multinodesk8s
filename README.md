# multinodesk8s
This playbook will install a multi-nodes k8s system

<bien@bienlab.com>

Before you start
----------------
On all nodes, create 'centos' user, adding public key and allow it to run as root without password
```
useradd centos
mkdir /home/centos/.ssh
chmod 700 /home/centos/.ssh
vi /home/centos/.ssh/authorized_keys
(adding your public key here)
chmod 400 /home/centos/.ssh/authorized_keys
chown -R centos:centos /home/centos/.ssh
echo "centos        ALL=(ALL)       NOPASSWD: ALL" > /etc/sudoers.d/centos
```
You may need to edit /etc/hosts and include all the node entries as the below:
```
192.168.31.11 node1 node1.bienlab.com
192.168.31.12 node2 node2.bienlab.com
192.168.31.13 node3 node3.bienlab.com
```
Disable SELinux as well (if you want)
```
sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/configCopy
setenforce 0
```
On the ansible controll machine, 
- modify the ansible.cfg file
- modify the hosts file
You may need to get all binary files (and yml as well) from here: https://github.com/biennt/aiok8s/tree/master/binary

Run they playbook
-----------------

Run the preparation playbook
```
ansible-playbook prepare_k8s.yml
```

Start the master node
```
ansible-playbook start_master.yml
```

Join the worker nodes
```
ansible-playbook join_workers.yml
```

Install network plugin and other stuff
```
ansible-playbook network_and_others.yml
```

# Ansible Playbooks To setup Kubernetes HA on Redhat Enterprise Linux 7.  

This repository provides Ansible Playbooks To setup Kubernetes HA on Redhat Enterprise Linux 7. The playbooks are mainly inspired by Kubeadm documentation and other ansible tentatives on github. The playbooks could be used separately or as one playbook for a fully fledged HA cluster. 

#Prerequisites: 

RHEL 7.2+
Install python pip:
```
wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -ivh epel-release-latest-7.noarch.rpm
yum install python-pip
```


Install ansible on your manager machine and on your workers

* You can do: 
```
pip install ansible
```

* Setup ssh access from master to workers.

# Environment preparation:

* Clone the repo:
 In the machine that you want to use as ansible master (can be your laptop or any other machine that has ssh access to the target machines):
 ```
 git clone git@github.com:IBM/ansible-kubernetes-ha-cluster.git
 cd ansible-k8s-ha
 ```

* Create inventory/mycluster
and declare your machines such as:
```
myhostname.domain.com ansible_usehost=<ip> ansible_user=<user>
```

There are different groups being defined and used, you can reuse mycluster file defined in inventory folder:
```
[all-masters] # these are all the masters
[all-workers] # these are all the worker nodes
[all-vms] # these are all vms of the cluster
```


You can check that you can ping all the machines:

```
ansible -m ping all -i inventory/mycluster
```
# Install a highly available kubernetes using kubeadm

You can now run k8s-all playbook to get your cluster setup.
You can also run the different playbooks separately for different purposes (setting up docker, etcd, keepalived, kubeadm ...).

```
ansible-playbook -i inventory/mycluster  playbooks/k8s-all.yaml
```

# What k8s-all.yaml includes:

- Installing docker
- Generating etcd certificates and installing ha etcd cluster on all the master nodes
- Installing keepalived and setting up vip management
- Installing kubeadm, kubelet and kubectl
- Setting up kubernetes masters
- Adding the nodes to the cluster
- Reconfiguring the nodes and components to use the vip

# Restarting the install:

If you need to restart the process using kubeadm reset, please use the cleanup-all-vms playbook that deletes the state from all vms. Some of the commands might fail but you can ignore that.

# Work in progress:
We will add the following items to this repository:
- Setting up firewalld in a more secure way
- Adding test playbooks
- Kubeadm upgrade playbook
- Adding the possibility of using other user then root
- Addons, Prometheus support?

# Contribution:

In order to contribute please feel free to create github issues and PRs.
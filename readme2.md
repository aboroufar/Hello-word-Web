In this guide, you will set up a Kubernetes cluster from scratch using Ansible and Kubeadm, your cluster will include the following physical resources:

### One master node
The master node (a node in Kubernetes refers to a server) is responsible for managing the state of the cluster.

### Two worker nodes
Worker nodes are the servers where your workloads (i.e. containerized applications and services) will run.

# Prerequisites
- Network configuration (IP address, DNS)
- Enabaled root account with the same password as indicated in playbook
- An SSH key pair on your local Linux/macOS/BSD machine (Ansible server).
 
	- To generate an RSA key pair on your local computer, type:

```sh
$ ssh-keygen
    
	Generating public/private rsa key pair.
    	Enter file in which to save the key (/home/demo/.ssh/id_rsa):
	Enter passphrase (empty for no passphrase):
    	Enter same passphrase again:
	
	Your identification has been saved in /root/.ssh/id_rsa.
	Your public key has been saved in /root/.ssh/id_rsa.pub.
	The key fingerprint is:
	8c:e9:7c:fa:bf:c4:e5:9c:c9:b8:60:1f:fe:1c:d3:8a root@here
	The key's randomart image is:
	+--[ RSA 2048]----+
	|                 |
	|                 |
	|                 |
	|       +         |
	|      o S   .    |
	|     o   . * +   |
	|      o + = O .  |
	|       + = = +   |
	|      ....Eo+    |
	+-----------------+
```
    
	
	This procedure has generated an RSA SSH key pair, located in the .ssh hidden directory within your user’s home directory. These files are:
	
	~/.ssh/id_rsa: The private key. DO NOT SHARE THIS FILE!
	~/.ssh/id_rsa.pub: The associated public key. This can be shared freely without consequence.
	
- Deployment environment must have Ansible `2.9.12+`
- Three servers running Ubuntu 18.04 with at least 4GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user.


# Usage

Setting Up the Workspace Directory and Ansible Inventory File
	
  	Add the system information including hostname and IP address gathered above into a file called `hosts.ini`. For example:
		
		[kubernetes_master_nodes]
		master01 ansible_host=192.167.18.10

		[kubernetes_worker_nodes]
		worker01 ansible_host=192.167.18.11
		worker02 ansible_host=192.167.18.12


	Before continuing, edit `group_vars/all.yml` to your specified configuration.

		For example, you are able to change the cluster IP address, and thus:

			api_addr: 192.167.18.10
			service_cidr: "10.96.0.0/12"
			pod_network_cidr: "10.10.0.0/16"

	**Note:** Depending on your setup, you may need to modify `cni_opts` to an available network interface. By default, `kubeadm-ansible` uses `eth1`. Your 			  default interface may be `eth0`.

# Installation

After going through the setup, run the `main-playbook.yaml` playbook:

```sh
$ ansible-playbook main-playbook.yaml

```

# Verification

Verify cluster is fully running, SSH into master node with the following command and using kubectl:

```sh
$ ssh ubuntu@master_ip

$ kubectl get node
NAME      STATUS    AGE       VERSION
master1   Ready     22m       v1.8.3
worker01  Ready     20m       v1.8.3
worker02  Ready     20m       v1.8.3

$ kubectl get po -n kube-system
NAME                                    READY     STATUS    RESTARTS   AGE
etcd-master01                            1/1       Running   0          23m
...
```

In this guide, you will set up a Kubernetes cluster from scratch using Ansible and Kubeadm,


Your cluster will include the following physical resources:

One master node
The master node (a node in Kubernetes refers to a server) is responsible for managing the state of the cluster.

Two worker nodes
Worker nodes are the servers where your workloads (i.e. containerized applications and services) will run.

Prerequisites

- An SSH key pair on your local Linux/macOS/BSD machine (Ansible server).
 
    - ssh-keygen
	
- Deployment environment must have Ansible `2.9.12+`

- Three servers running Ubuntu 18.04 with at least 4GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user.

 
Step 1 — Setting Up the Workspace Directory and Ansible Inventory File


Build a Kubernetes cluster using Ansible with kubeadm. The goal is easily install a Kubernetes cluster on machines running:

  - Ubuntu 18.04
  
  
System requirements:

  - Deployment environment must have Ansible `2.9.12+`
  - Master and nodes must have SSH access

# Usage

Add the system information gathered above into a file called `hosts.ini`. For example:
```
[master]
192.168.17.10

[node]
192.168.17.11

```

```

Before continuing, edit `group_vars/all.yml` to your specified configuration.

For example, you are able to change the cluster IP address, and thus:

api_addr: 192.167.18.10
service_cidr: "10.96.0.0/12"
pod_network_cidr: "10.10.0.0/16"

**Note:** Depending on your setup, you may need to modify `cni_opts` to an available network interface. By default, `kubeadm-ansible` uses `eth1`. Your default interface may be `eth0`.



After going through the setup, run the `site.yaml` playbook:

```sh
$ ansible-playbook main-playbook.yaml

```

```sh


Verify cluster is fully running, SSH into master node with the following command and using kubectl:

```sh
$ ssh ubuntu@master_ip

```

```sh


```
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

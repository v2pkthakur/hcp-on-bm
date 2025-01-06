# hcp-on-bm
HCP on BareMetaal

This Repo will host the process for deploying a Hosted Control Plane cluster on a Hub Cluster installed on a single bare metal node. 

![High-Level-Arch](images/hcp-on-bm.png)


## Getting Started

Start with freshly installed RHEL9 OS on the BM. And make sure you have valid subscriptions. 

```
subscription-manager register
yum install ansible-core -y
ansible-galaxy collection install community.libvirt
ansible-galaxy collection install community.crypto
```

Also download `rhel-9.4-x86_64-kvm.qcow2` from access.redhat.com/downloads. 

Pull the Ansible Repo

```
git clone https://github.com/v2pkthakur/hcp-on-bm.git
cp rhel-9.4-x86_64-kvm.qcow2 hcp-on-bm/roles/setup-bm-host/files/

cd hcp-on-bm
ansible-playbook playbook.yaml
```

Lets start with setting up the Bare Metal Host. 
```
[root@base hcp-on-bm]# ansible-playbook -i inventory/hosts setup_bm_host.yaml   --ask-vault-pass
Vault password:
```
The above playbook will create and configure a virtual machine named `helper` which will be hosting the DNS and HAProxy for the setup. 

Once we have that ready, lets start setting up the Hub Cluster. 
```
[root@base hcp-on-bm]# ansible-playbook -i inventory/hosts setup_hub_cluster.yaml   --ask-vault-pass
Vault password:
```

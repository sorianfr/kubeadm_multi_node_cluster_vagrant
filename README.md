# kubeadm_multi_node_cluster_vagrant

This project provides a Vagrant setup for creating a multi-node Kubernetes cluster using kubeadm.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

1. **VirtualBox**  
   VirtualBox is a free and open-source virtualization software. You can download it from the official site:  
   [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)

2. **Vagrant**  
   Vagrant is a tool for building and managing virtualized development environments. You can download it from the official site:  
   [Download Vagrant](https://www.vagrantup.com/downloads)

## Installation

1. Download and install VirtualBox and Vagrant using the links provided above.
2. Clone this repository to your local machine:

   ```bash
   git clone https://github.com/sorianfr/kubeadm_multi_node_cluster_vagrant.git

3. Navigate to the project directory:

   ```bash
   cd kubeadm_multi_node_cluster_vagran
   
4. Run the vagrantfile to start building the VMs

   ```bash
   vagrant up

6. Once the 3 VMs are up & running we copy setup_k8s.sh file onto each machine. For this we use 

   ```bash
   vagrant plugin install vagrant-scp   
   vagrant scp setup_k8s.sh master:/home/vagrant/setup_k8s.sh 
   vagrant scp setup_k8s.sh nodo01:/home/vagrant/setup_k8s.sh 
   vagrant scp setup_k8s.sh nodo02:/home/vagrant/setup_k8s.sh 

   On each machine:
   ```bash
   vagrant ssh <name_of_VM>
   chmod +x /home/vagrant/setup_k8s.sh
   sudo /home/vagrant/setup_k8s.sh 

   This will follow the instructions from Kubernetes guide to setup the cluster using kubeadm
   
7. Once finished, we initialize the cluster with:
   ```bash
   sudo kubeadm init --apiserver-advertise-address=192.168.86.103 --pod-network-cidr=192.168.0.0/16 

8. It will prompt the following message:

   Your Kubernetes control-plane has initialized successfully!

   To start using your cluster, you need to run the following as a regular user:

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config

9. You should now deploy a pod network to the cluster. We will use Calico

   Quickstart for Calico on Kubernetes | Calico Documentation (tigera.io) 
   ```bash
   sudo kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/tigera-operator.yaml 
   sudo kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/custom-resources.yaml 

10. We join the other nodes.. with sudo kubeadm join...

    

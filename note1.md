K8s Features 
	Container Orchestration 
		The primary purpose of Kubernetes is to dynamically manage containers across multiple host systems. 
	Application Reliability 
		Kubernetes makes it easier to build reliable, self-healing, and scalable applications. 
	Automation 
		Kubernestes offers a variety of features to help automate the management of your container apps.

K8 Control Plan 
	The control plane is a collection of multiple components responsible for managing the cluster itself globally. Essentially, the control plan controls the cluster. 
	
	Individual control plane components can run on any machine in the cluster, but usually are run on dedicated controller machines. 

	kube-api-server: 
		-  serves the Kubernetes API, the primary interface to the control plane and the cluster itself. 
		- When interacting with your Kubernetes cluster, you will usually do so using the Kubernetes API. 

	Etcd:
		- Etcd is the backend data store for the Kubernetes cluster. It provies high-availability storage for all data relating to the state of the cluster. 

	kube-scheduler: 
		- kube-scheduler handles scheduling, the process of selecting an available node in the cluster on which to run containers. 

	kube-controller-manager: 
		- kube-controller-manager runs a collection of multiple controller utilities in a single process. These controllers carry out a variety of automation-related tasks within the Kubernetes cluster. 

	cloud-controller-manager: 
		- cloud-controller-manager provides an interface between Kubernetes and various cloud platforms. It is only used when using cloud-based resources alongside Kubernetes. 

K8s Nodes 
	Nodes: 
		- Kubernetes Nodes are the machines where the containers managed by the cluster run. A cluster can have any number of nodes. 
		- Various node components manage containers on the machine and communicate with the control plane. 

		kubelet: 
			- Kubelet is the Kubernetes agent that runs on each node. It communicates with the control plane and ensures the containers are run on its node as instructed by the control plane. 
			- Kubelet also handles the process of reporting container status and other data about containers back to the control plane. 
i
		container runtime: 
			- The container runtime is not built into Kubernetes. It is a separate piece of software that is responsible for actually running containers on the machine. 
			- Kubernetes supports multiple container runtime implementations. Some popular container runtimes are Docker and containerd. 

		kube-proxy:
			- kube-proxy is a network proxy. It runs on each node and handles some tasks related to providing networking between containers and services in the cluster. 

Building a Kubernetes Cluster: 

	kubeadm
	Playground Server Setup 
	Hands-on

kubadm:
	- You can simplify the process of building a cluster by using kubeadm to configure your control plane and worker nodes. 
	- 3 servers - ubuntu 18.04 

Hostname: k8s-control 
- cat << EOF | sudo tee /etc/modules-load.d/containerd.conf 
overlay
br_netfilter 
EOF

#### to load without reboot
sudo modprobe overlay 
sudo modprobe br_netfilter 

cat << EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf 
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1 
net.bridge.bridge-nf-call-ip6tables = 1 
EOF 

sudo sysctl --system   (apply those settings immediately)

sudo apt update ** sudo apt install -y containerd 

sudo mkdir -p /etc/containerd

sudo containerd config default | sudo tee /etc/containerd/config.toml 

sudo systemctl restart containerd 

sudo swappoff -a   

sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/stab   (need to disable swap)

sudo apt update && sudo apt install -y apt-transport-https curl 

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add - 

cat < EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list 

sudo apt update 

sudo apt install -y kubelet=1.20.1-00 kubeadm=1.20.1-00 kubectl=1.20.1-00

sudo apt-mark hold kubelet kubeadm kubectl  (hold automatic update)

 repeat above steps on both the worker nodes

Only on control node

sudo kubeadm init --pod-network-cidr 192.168.0.0/16 

	mkdir -p $HOME/.kube 
	sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
	sudo chown $(id -u):$(id -g) $HOME/.kube/config 

kubectl version 

To setup K8s network 

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml 

To verify 

kubectl get pods -n kube-system 

Worker node to join the cluster 

kubeadm token create --print-join-command 

	the output is the token that should run on the worker node to join the network cluster 
	sudo and token node 

on control

kubectl get nodes 

## Chapter 3 








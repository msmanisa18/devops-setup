### Installing kubeadm:

#### System Requirement:
	2 GB or more of RAM per machine
	2 CPUs or more for control plane machine

#### According to the above requirements, launch 3 EC2 Ubuntu instances.

#### Run following commands on master:
		sudo su
		hostnamectl set-hostname master
		bash
		hostname

#### Run following commands in node-1:		
		sudo su
		hostnamectl set-hostname node-1
		bash
		hostname	

#### Run following commands in node-2:
		sudo su
		hostnamectl set-hostname node-2
		bash
		hostname

#### Run following commands in all the 3 machines:

	1. apt-get update
	
	2. apt-get install -y apt-transport-https ca-certificates curl gpg
	
	3. apt-get install docker.io -y

	4. systemctl start docker
	
	5. systemctl enable docker
	
	6. curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
	
	7. echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
	
	8. curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

	9. apt-get update
	
	10. apt-get install -y kubelet kubeadm kubectl
	
	11. apt-mark hold kubelet kubeadm kubectl

#### Run following commands on master:

	1. kubeadm init
 		or --> kubeadm init --pod-network-cidr=192.168.0.0/16

	2. mkdir -p $HOME/.kube
	
	3. sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
	
	4. sudo chown $(id -u):$(id -g) $HOME/.kube/config
	
	5. kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
		
#### Run following commands in node-1 and node-2:

	Copy the join command from master and execute on both the nodes.
	Example: kubeadm join 172.31.35.99:6443 --token jq35cs.clzt95g3zndlmjj6 \
        --discovery-token-ca-cert-hash sha256:2a882906da2d197032e3a980fa6a36b10a983b45bc75efe52228872a98abd5f9

#### Run following commands on master:
	kubectl get nodes
	kubectl get nodes -o wide
	kubectl get pods -o wide
	kubectl get pods
	kubectl get svc
	kubectl get deployment


### Flannel
 	vi /run/flannel/subnet.env

	FLANNEL_NETWORK=10.240.0.0/16
	FLANNEL_SUBNET=10.240.0.1/24
	FLANNEL_MTU=1450
	FLANNEL_IPMASQ=true


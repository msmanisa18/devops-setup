### Installing kubeadm:

#### Minimum Requirements for Master:
	Ubuntu - t2.medium - 15 GB SSD

#### Minimum Requirements for Worker Node:
	Ubuntu: t2.small - 15 GB RAM

#### Run following commands in Master.
	sudo su --> switch user to root
	hostnamectl set-hostname master --> To change the hostname of Linux system to Master
	bash --> Bourne Again Shell, allows users to interact with a Linux operating system
	hostname --> A hostname is a unique name or label given to a device connected to a computer network. 

#### Run following commands in Node-1.
	sudo su
	hostnamectl set-hostname node-1
	bash
	hostname

#### Run following commands in Node-2.
	sudo su
	hostnamectl set-hostname node-2
	bash
	hostname

#### Run following commands in All the VMs.

	1. apt-get update  --> updates the apt package index files
	
	2. apt-get install -y apt-transport-https ca-certificates curl gpg  --> to install software packages needed to use the Kubernetes apt repository using the Advanced Package Tool

	3. apt-get install docker.io -y --> Install Docker, by default Docker will be there on Ubuntu image.

	4. systemctl start docker --> To start Docker Enginr

	5. systemctl enable docker --> Enables the Docker service in Linux

	6. curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
		--> Required for intra cluster communication, Master will communicate with the nodes.
		--> So, we need to install the key.gpg from Google cloud.
	
	7. echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
		--> Download K8s packages
	
	8. curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
		--> Download the public signing key for the Kubernetes package repositories.

	9. apt-get update --> updates the apt package index files
	
	10. apt-get install -y kubelet kubeadm kubectl --> Install kubelet, kubeadm and kubectl

	11. apt-mark hold kubelet kubeadm kubectl  --> prevents the K8s components from being upgraded, allowing them to stay at a specific version

#### Run following command in Master.

	1. kubeadm init --pod-network-cidr=10.244.0.0/16
		--> Initialize and mention the CIDR ( Network range ) for the K8S cluster.

#### Run following command in Node-1 and Node-2.
	
	1. kubeadm join 172.31.47.35:6443 --token 3euqbc.jqgbu3ll4od6xgy3 \
        --discovery-token-ca-cert-hash sha256:e7b68fad0c01b6476712775369f912e6ee43f0c6d3d265a1207bd0b7c328df81
		
		--> makes the machines as worker nodes

#### Run following commands in Master.

	1. mkdir -p $HOME/.kube

	2. cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

	3. chown $(id -u):$(id -g) $HOME/.kube/config

	4. kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

#### Create a deployment on master:

#### vi mydeployment.yml

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  selector:
    matchLabels:
      run: load-balancer-example
  replicas: 2
  template:
    metadata:
      labels:
        run: load-balancer-example
    spec:
      containers:
        - name: hello-world
          image: manisa2024/hello-spring-boot:v1
          ports:
            - containerPort: 8085
              protocol: TCP
...

#### 1. kubectl apply -f mydeployment.yml

#### Display information about the Deployment:
	kubectl get deployments hello-world
	kubectl describe deployments hello-world

#### Display information about your ReplicaSet objects:
	kubectl get replicasets
	kubectl describe replicasets

#### Create a Service object that exposes the deployment:
	kubectl expose deployment hello-world --type=NodePort --name=example-service

#### Display information about the Service:
	kubectl describe services example-service

#### It should return like:

	root@master:/home/ubuntu# kubectl describe services example-service
	Name:                     example-service
	Namespace:                default
	Labels:                   <none>
	Annotations:              <none>
	Selector:                 run=load-balancer-example
	Type:                     NodePort
	IP Family Policy:         SingleStack
	IP Families:              IPv4
	IP:                       10.106.213.205
	IPs:                      10.106.213.205
	Port:                     <unset>  8085/TCP
	TargetPort:               8085/TCP
	NodePort:                 <unset>  30324/TCP
	Endpoints:                10.244.1.6:8085,10.244.3.8:8085
	Session Affinity:         None
	External Traffic Policy:  Cluster
	Internal Traffic Policy:  Cluster
	Events:                   <none>
	root@master:/home/ubuntu#

#### List the pods that are running the Hello World application:
	kubectl get pods --selector="run=load-balancer-example" --output=wide

#### Get the public IP address of one of your nodes that is running a Hello World pod. 
#### Use the node port to access the Hello World application (From above example - 30324).
#### And then access the application.

## Thank you!!!

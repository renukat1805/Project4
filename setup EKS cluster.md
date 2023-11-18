Install EKS-server
--------------------
* launch a ec2 machine (ubuntu - recomended)
* create a IAM role with AdministrationAccess and attach with ec2 machine
* install kubectl on ec2 machine

		curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.25.6/2023-01-30/bin/linux/amd64/kubectl
		chmod +x ./kubectl
		mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
		kubectl version --short --client

install eksctl
--------------- 
	
		curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
		sudo mv /tmp/eksctl /usr/local/bin
		eksctl version 

create eks cluster with command 
--------------------------------

		eksctl create cluster --name thej-cluster --node-type t2.large --nodes 3 --region ap-southeast-1
	
* from above command to create cluster with 3 machines in singapoor region 
* it takes minimum 15 minuites to create cluster 
* After cluster creation to login into cluster it needs a pluggin called "aws-iam-authenticator pluggin" (when launch amazon linux it will not ask plugin setup)  
	
		curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
		chmod +x ./aws-iam-authenticator
		mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin
		echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
		aws-iam-authenticator help


To check Nodes 
---------------
	kubectl get nodes

>To delete cluster
	
		eksctl delete cluster thej-cluster 
		eksctl delete cluster thej-cluster --region ap-southeast-1

* above command delete cluster and it will take few minuites to delete 

>syntax of cluster creation

		eksctl create cluster --name=thej-cluster --region=ap-southeast-1 --node-type=<node-instance-type> --nodes=<number-of-nodes>

> cluster creation command

		eksctl create cluster --name=thej-cluster --region=ap-southeast-1 --node-type=t2.large --nodes=3

>cluster deletion

		eksctl delete cluster --name=thej-cluster --region=ap-southeast-1



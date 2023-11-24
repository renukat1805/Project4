Kubernetes Cluster Setup


Installation of EKS-server
--------------------
* Launch a ec2 machine (ubuntu)
* Create an IAM role with AdministrationAccess and attach with ec2 machine
* Install kubectl on ec2 machine
  
		curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/linux/amd64/kubectl
                chmod +x ./kubectl
		mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
                kubectl version --client


Installation of eksctl
--------------------
	
		# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"

# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check

tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz

sudo mv /tmp/eksctl /usr/local/bin

  eksctl version 

create eks cluster with command 
--------------------------------

		eksctl create cluster --name mycluster --node-type t2.large --nodes 3 --region us-east-2
	
* Created cluster with 3 machines in Ohio region from the above command 
* After cluster creation, to login into cluster it needs a plugin called "aws-iam-authenticator pluggin" 
	
		curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.6.11/aws-iam-authenticator_0.6.11_linux_amd64
                chmod +x ./aws-iam-authenticator
		mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH
                echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
                aws-iam-authenticator help


To check Nodes 
---------------
	kubectl get nodes

>To delete cluster
	
	eksctl delete cluster mycluster 
	eksctl delete cluster mycluster --region us-east-2

>syntax of cluster creation

		eksctl create cluster --name mycluster --region=us-east-2 --node-type=<node-instance-type> --nodes=<number-of-nodes>

> cluster creation command

		eksctl create cluster --name mycluster --region=us-east-2 --node-type=t2.large --nodes=3

>cluster deletion

		eksctl delete cluster --name mycluster --region=us-east-2
  * Configuring of YAML files for kubernetes deployment
  * create deployment manifest file for 2 replicas and includes resources with Nginx image
  * created service maniest file for any reason if pod gets crashed the newly created pod will contain the same IP address as of the deleted pod
  * Also included the type Loadbalancer and assign selector field with labels, as it should be applied for any pod which is having the labels(type: proxy)
  * 
  



# aws-eks-deploy-nginx-server
using aws eks to deploy nginx server
# 2048-game-app-deploy-aws-eks
deployment of 2048 game app on aws eks cluster on aws infrastructure
# Requirement
--Make sure to have an EC2 instance of t3 medium, Ubuntu operating system
--IAM role with administrative access to modify the EC2 instance IAM role
--Create an IAM user with administrative access permission in AWS, with an access key and secret key
--Install AWS CLI on the EC2 instance
--Confirm installation by running the command "aws --version"
--Authenticate with AWS by running the command "aws configure"
--Insert access key, secret key, region and json format
--Confirm authentication by running the command "aws s3 ls"
--Install kubectl
--Install eksctl and confirm installation
 
 # Installation of script
 #!/bin/bash
# install eksctl

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version
#==================================
## install aws cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" 
sudo apt install unzip 
unzip awscliv2.zip 
sudo ./aws/install

#check the version

aws --version
#======================================
#now check if you have kubectl on your machine, run the command below
$kubectl version
$kubectl version --client

#if not exist the install kubectl on linux used the link below

https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux


#give permissions to kubectl

chmod +x kubectl

#Move the kubectl into the usr/local/bin
$mv kubectl /usr/local/bin

#verfy the installation of kubectl

$kubectl version

#or if the above does not installed

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version

# Installation and configuration of AWS EKS (and AWS kubernetes cloud manage service)
eksctl create cluster --name aws-eks-nginx --node-type t3.medium --nodes 1 --nodes-min 1 --nodes-max 2 --region us-east-1

# Output of my AWS configuration
![Image](https://github.com/user-attachments/assets/2115a552-9e3f-42d6-a523-0a9c8d7e3366)

## Get EKS Cluster service
eksctl get cluster --name aws-eks-nginx --region us-east-1

# Update kubconfig
aws eks update-kubeconfig --name aws-eks-nginx --region us-east-1

## Get EKS Pod data.
kubectl get pods --all-namespaces

# Next step is to create 2 yaml files and input the scripts
nginx-deployment.yaml nginx-service.yaml

# Apply the deployment file
kubectl apply -f nginx-deployment.yaml
# Get pods
kubectl get pods
# Apply the service file
kubectl apply -f nginx-service.yaml

# Check service
kubectl get svc

# Check the link locally
copy the loadbalancer link that ends with .com
Type curl (paste link)

# Output of the deployment
![Image](https://github.com/user-attachments/assets/566e0426-99ed-4b55-abd8-2a298b8cf270)

![Image](https://github.com/user-attachments/assets/bc8bd065-c69f-4dd7-a6f5-008e66cb4a1c)

# Output on the browser
![Image](https://github.com/user-attachments/assets/07599604-7950-4e26-ab81-fd6e25c616d7)

# How to insert into a pod (the pod must be running)
kubectl exec -it "podname" -- sh



# Process of cleaning up
kubectl delete svc mygame-svc
kubectl delete -f nginx-deployment.yaml

## Delete EKS cluster
eksctl delete cluster --name aws-eks-nginx --region us-east-1

# Final output of cleaning up
![Image](https://github.com/user-attachments/assets/5098715a-c145-4096-a3c2-04dd5d38bb46)
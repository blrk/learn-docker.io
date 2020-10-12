### setup Kubernetes (K8s) Cluster on AWS
#### Create Ubuntu EC2 instance
* create instance : ubuntu 18 image
* type : t2.micro
* Tag : Name; k8-management-server
#### Basic Configuration
* Login as root user
``` bash
sudo su -
```
* update the instance
``` bash
apt update -y
```
* Install zip command
``` bash
apt install zip -y 
```
* install python
``` bash
apt install python -y
```
#### install AWSCLI
``` bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
```
* Verify the installation
``` bash
aws --version
aws-cli/2.0.56 Python/3.7.3 Linux/5.3.0-1035-aws exe/x86_64.ubuntu.18
```
#### Install kubectl on ubuntu instance
``` bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
#### Install kops on ubuntu instance
``` bash
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```
#### Create an IAM role and attach to the instance
* Open AWS console
* click on Services
* Select the IAM service
* Click on Roles
* Select EC2
* In the Filter policies : Search the following
* AmazonEC2FullAccess : select it
* AmazonS3FullAccess : select it
* AmazonRoute53FullAccess : select it
* IAMFullAccess : Select it
* Click in Next Tags button
* In the Add tags
* Key : Name
* value : k8-management-server-role
* Click Next: Review button
* Give the role name : k8-management-server-role
* click on Create a role 
* Move to the EC2 Running isntances dashboard
* select the : k8-management-server server and attach the role

#### Attach IAM role to ubuntu instance
``` bash
# Note: If you create IAM user with programmatic access then provide Access keys. Otherwise region information is enough
aws configure
```
#### Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)
``` bash
Routeh53 --> hosted zones --> created hosted zone  
Domain Name: karunya.edu
Type: Private hosted zone for Amzon VPC
```
#### create an S3 bucket
``` bash
aws s3 mb s3://demo.k8s.karunya.edu
```
#### Expose environment variable:
``` bash
 export KOPS_STATE_STORE=s3://demo.k8s.karunya.edu
 ```
#### Create sshkeys before creating cluster
``` bash
ssh-keygen
```
#### Create kubernetes cluster definitions on S3 bucket
* find the zone of your instance
* From the terminal of your k8-management-server run the following command
``` bash
curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone
```
``` bash
kops create cluster --cloud=aws --zones=ap-south-1a --name=demo.k8s.karunya.edu --dns-zone=karunya.edu --dns private 
```
#### Create kubernetes cluser
``` bash
kops update cluster demo.k8s.karunya.edu --yes
```
#### Validate your cluster
``` bash
kops validate cluster
```
#### To list nodes
``` bash
kubectl get nodes
```
#### To delete cluster
``` bash
kops delete cluster demo.k8s.karunya.edu --yes
```
### Deploying Nginx pods on Kubernetes
#### Deploying Nginx Container
``` bash
kubectl run sample-nginx --image=nginx --replicas=2 --port=80
# kubectl run simple-devops-project --image=blrk/devops-website-image --replicas=2 --port=8080
kubectl get pods
kubectl get deployments
```
#### Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them.
``` bash
kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
# kubectl expose deployment devops-website-image --port=8080 --type=LoadBalancer
kubectl get services -o wide
```

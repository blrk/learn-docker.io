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
#### If you face any issues in cluster creation install the following version
``` bash
 curl -LO  https://github.com/kubernetes/kops/releases/download/1.15.0/kops-linux-amd64
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
#### How to modify the cluster configuration before creating the cluster
``` bash
 * list clusters with: kops get cluster
 * edit this cluster with: kops edit cluster demo.k8s.karunya.edu
 * edit your node instance group: kops edit ig --name=demo.k8s.karunya.edu nodes
 * edit your master instance group: kops edit ig --name=demo.k8s.karunya.edu master-ap-south-1b
```
#### Create kubernetes cluser
``` bash
kops update cluster demo.k8s.karunya.edu --yes
```
#### Validate your cluster
``` bash
kops validate cluster
```
* Note : You get the following error if your cluster creation is not completed
``` 
Validation failed: cluster not yet healthy
```
* After Cluster creation is done, run the following command
``` bash
kops validate cluster
Using cluster from kubectl context: demo.k8s.karunya.edu

Validating cluster demo.k8s.karunya.edu

INSTANCE GROUPS
NAME			ROLE	MACHINETYPE	MIN	MAX	SUBNETS
master-ap-south-1b	Master	t3.medium	1	1	ap-south-1b
nodes			Node	t3.medium	2	2	ap-south-1b

NODE STATUS
NAME						ROLE	READY
ip-172-20-58-240.ap-south-1.compute.internal	master	True
ip-172-20-61-135.ap-south-1.compute.internal	node	True
ip-172-20-61-2.ap-south-1.compute.internal	node	True

Your cluster demo.k8s.karunya.edu is ready
```
#### To list nodes
``` bash
kubectl get nodes
```
* output
``` bash
kubectl get nodes
NAME                                           STATUS   ROLES    AGE   VERSION
ip-172-20-58-240.ap-south-1.compute.internal   Ready    master   14m   v1.18.9
ip-172-20-61-135.ap-south-1.compute.internal   Ready    node     12m   v1.18.9
ip-172-20-61-2.ap-south-1.compute.internal     Ready    node     12m   v1.18.9
```
#### To delete cluster
``` bash
kops delete cluster demo.k8s.karunya.edu --yes
```
* Suppose, the above command is not working use the following command
``` bash
kops delete cluster --name=demo.k8s.karunya.edu --state=s3://demo.k8s.karunya.edu --yes
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
[Back to the mainpage](https://github.com/blrk/learn-devops.io/wiki)

## Setup Kubernetes (K8s) Cluster on AWS


### Create Ubuntu EC2 instance
### Basic Configuration
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
./aws/install -i /usr/local/aws -b /usr/local/bin/aws
```
```
    sudo apt-get update
    
    apt install unzip python
    unzip awscli-bundle.zip
    #sudo apt-get install unzip - if you dont have unzip in your system
    ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    ```

#### Install kubectl on ubuntu instance
   ```sh
   curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
   ```

#### Install kops on ubuntu instance
   ```sh
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```
#### Create an IAM user/role  with Route53, EC2, IAM and S3 full access

#### Attach IAM role to ubuntu instance
   ```sh
   # Note: If you create IAM user with programmatic access then provide Access keys. Otherwise region information is enough
   aws configure
    ```

#### Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)
   ```sh
   Routeh53 --> hosted zones --> created hosted zone  
   Domain Name: karunya.edu
   Type: Private hosted zone for Amzon VPC
   ```

#### create an S3 bucket
   ```sh
    aws s3 mb s3://demo.k8s.karunya.edu
   ```
#### Expose environment variable:
   ```sh
    export KOPS_STATE_STORE=s3://demo.k8s.karunya.edu
   ```

#### Create sshkeys before creating cluster
   ```sh
    ssh-keygen
   ```

#### Create kubernetes cluster definitions on S3 bucket
   ```sh
   kops create cluster --cloud=aws --zones=ap-south-1b --name=demo.k8s.karunya.edu --dns-zone=karunya.edu --dns private 
    ```

#### Create kubernetes cluser
    ```sh
    kops update cluster demo.k8s.karunya.edu --yes
    ```

#### Validate your cluster
     ```sh
      kops validate cluster
    ```

#### To list nodes
   ```sh
   kubectl get nodes
   ```

#### To delete cluster
    ```sh
     kops delete cluster demo.k8s.karunya.edu --yes
    ```
   
### Deploying Nginx pods on Kubernetes
#### Deploying Nginx Container
    ```sh
    kubectl run sample-nginx --image=nginx --replicas=2 --port=80
    # kubectl run simple-devops-project --image=blrk/devops-website-image --replicas=2 --port=8080
    kubectl get pods
    kubectl get deployments
   ```

#### Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them.
   ```sh
   kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
   # kubectl expose deployment devops-website-image --port=8080 --type=LoadBalancer
   kubectl get services -o wide
   ```

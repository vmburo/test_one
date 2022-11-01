# Steps

# Requirements installation
eksctl,
kubectl,
awscli,
helm,

You need to configure awscli with your own access key and secret key (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html)

# AWS EKS Setup
Setup kubectl
a. Download kubectl version 1.20
b. Grant execution permissions to kubectl executable
c. Move kubectl onto /usr/local/bin
d. Test that your kubectl installation was successful

curl -o kubectl https://amazon-eks.s3.us-east-1.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin 
kubectl version --short --client

# Setup eksctl
a. Download and extract the latest release
b. Move the extracted binary to /usr/local/bin
c. Test that your eksclt installation was successful

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

# Setup AWS CLI
#curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
#curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

# Setup helm

#curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
#chmod 700 get_helm.sh
#./get_helm.sh

# configure awscli with your own access key and secret key

Access key ID  AKIAWRMZT4EKXAB6ZMJB

Secret access key   xTNgy8+sPDKypR7rm9SBfMrFF9PnSdXJebBtOCDc

aws configure (provide the credentials)

# Create EKS Cluster

#eksctl create cluster --name test-cluster --region us-east-1

Wait for the command to finish then verify cluster by executing 

#kubectl get nodes

# Deploy containers
#kubectl apply -f nginx.yaml

#kubectl apply -f nginx-whoami.yaml

# Verify pods in running state

#kubectl get pod

# Deploy Nginx Ingress Controller

#helm -n ingress-nginx install ingress-nginx  ingress-nginx/ingress-nginx --create-namespace

Verify Nginx pod running

#kubectl get pod -n ingress-nginx

# Verify Load Balancer created by controller

#kubectl get svc -n ingress-nginx

Output should look like

#kubectl get svc -n ingress-nginx

NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.100.20.221    a6f752d8ef7a94ddfbb0c7123619a4ce-300370210.us-east-1.elb.amazonaws.com   80:30649/TCP,443:32755/TCP   8h
ingress-nginx-controller-admission   ClusterIP      10.100.100.107   <none>  


# Deploy Ingress objects

#kubectl apply -f ingress.yaml

# Verify ingress by accessing above load balancer dns.

Accessing to http://a6f752d8ef7a94ddfbb0c7123619a4ce-300370210.us-east-1.elb.amazonaws.com/ should return "Nginx Main" - which is the index file of first Nginx container

#Accessing to http://a6f752d8ef7a94ddfbb0c7123619a4ce-300370210.us-east-1.elb.amazonaws.com/ should return "Nginx Main" - which is the index file of first Nginx container

#Acccessing to http://a6f752d8ef7a94ddfbb0c7123619a4ce-300370210.us-east-1.elb.amazonaws.com/whoami/ should return "Nginx WhoAmI" - which is the index file of whoami container

# Terraform Requirements
#Install Terraform

Verify your aws account:
#aws sts get-caller-identity


# Execute terraform

Update value of user and s3 bucket in terraform.tfvars file

Initialize providers
#terraform init

Run plan
#terraform apply

After terraform executed successfully, get output
#terraform output access_key
#terraform output secret_key

#Export these two values with env vars:

#export AWS_ACCESS_KEY_ID=<access_key>

#export AWS_SECRET_ACCESS_KEY=<secret_key>

# Verify current user is one created by terraform after exported env vars

#aws sts get-caller-identity


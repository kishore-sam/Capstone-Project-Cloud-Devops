
Capstone Project

Steps:


Step 1: Create Infrastructure amd EKS Stack

1. aws cloudformation create-stack --stack-name EKSInfraStack --region us-west-2 --template-body file://ourinfra.yml --parameters file://ourinfra-params.json

2. aws cloudformation create-stack --stack-name EKSClusterStack --region us-west-2 --template-body file://eks-cluster.yml --parameters file://eks-cluster-params.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"


Step 2: Set up pipeline

1. Create an ec2 instance in AWS Console 
2. Install and set up jenkins in the newly created ec2 instance along with necessary plugins - Blue Ocean, Pipeline-aws
3. Install necessary packages in ec2 instance - tidy, docker, awscli, kubectl
4. Initialize kubernetes cluster from ec2 instance
        Run: aws eks --region us-west-2 update-kubeconfig --name EKSCluster

Reference:
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html
https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html

Note: The user jenkins needs to be added to the group docker (to fix permission denied error).
Run Commands:
sudo groupadd docker
sudo usermod -a -G docker jenkins
grep docker /etc/group


Step 3: Build Docker Image and Deploy Container in Kubernetes

1. Lint stage: Uses tidy plugin to check for errors in index.html file.
2. Build stage: Uses Dockerfile to build a docker image and push to docker hub repository.
3. Deploy stage: Deploy the docker image in kubernetes cluster (EKS).


Step 4: Test pipeline

1. Introduce bugs in index.html file to check linting errors.
2. Remove errors and check linter passes.
3. Create a new docker image and check for rolling deployment in kubernets pods.


Step :Delete Stack

1. aws cloudformation delete-stack --stack-name EKSClusterStack
2. aws cloudformation delete-stack --stack-name EKSInfraStack
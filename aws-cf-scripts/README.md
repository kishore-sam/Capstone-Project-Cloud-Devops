
Capstone Project

Create Stack:
aws cloudformation create-stack --stack-name EKSInfraStack --region us-west-2 --template-body file://ourinfra.yml --parameters file://ourinfra-params.json

aws cloudformation create-stack --stack-name EKSClusterStack --region us-west-2 --template-body file://eks-cluster.yml --parameters file://eks-cluster-params.json --capabilities "CAPABILITY_IAM" "CAPABILITY_NAMED_IAM"

Delete Statck:
aws cloudformation delete-stack --stack-name EKSClusterStack
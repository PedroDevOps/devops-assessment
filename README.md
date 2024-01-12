### Create CloudFormation Stacks

```
aws cloudformation create-stack --template-body file://$PWD/vpc.yml --stack-name vpc

aws cloudformation create-stack --template-body file://$PWD/ecs-cluster.yml --stack-name ecs-cluster --capabilities CAPABILITY_IAM

aws cloudformation create-stack --template-body file://$PWD/container-with-auto-scalling.yml --stack-name container-with-auto-scalling --capabilities CAPABILITY_IAM



aws cloudformation delete-stack --stack-name vpc

aws cloudformation delete-stack --stack-name ecs-cluster

aws cloudformation delete-stack --stack-name container-with-auto-scalling
```
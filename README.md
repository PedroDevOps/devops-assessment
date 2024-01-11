### Create CloudFormation Stacks

```
aws cloudformation create-stack --template-body file://$PWD/infra/vpc.yml --stack-name vpc

aws cloudformation create-stack --template-body file://$PWD/ecs-cluster.yml --stack-name ecs-cluster --capabilities CAPABILITY_IAM

aws cloudformation create-stack --template-body file://$PWD/infra/cluster.yml --stack-name app-cluster

aws cloudformation create-stack --template-body file://$PWD/infra/aplication.yml --stack-name api
```
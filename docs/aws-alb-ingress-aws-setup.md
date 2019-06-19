
# Setting up NGINX Ingress Controller in AWS 

Reference:
https://aws.amazon.com/blogs/opensource/kubernetes-ingress-aws-alb-ingress-controller/

## Steps

1. cd aws-alb-ingress-setup

2. Create policy and attach it to NodeInstanceRole
   Use iam-policy.json to create the policy.
  

3. Apply  rbac-role.yaml
```sh
kubectl apply -f rbac-role.yaml
```

3. Apply  alb-ingress-controller.yaml
```sh
kubectl apply -f alb-ingress-controller.yaml
```






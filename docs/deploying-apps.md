
# Deploying the Fleetman application

Prerequisite:
```sh
cd ~/environment
git clone https://github.com/oskiegarcia/aws-eks-demo.git
cd aws-eks-demo
```

1. apply services
```sh
kubectl apply -f services.yaml
```

2. apply workloads
```sh
kubectl apply -f workloads.yaml
```

3. verify that pods are runnning
```sh
kubectl get all
```





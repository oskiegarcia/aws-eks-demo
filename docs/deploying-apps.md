
# Deploying the Fleetman application

Prerequisite:
- Get yaml configs from github
```sh
cd ~/environment
git clone https://github.com/oskiegarcia/aws-eks-demo.git
cd aws-eks-demo
```


### Steps

1. Create PVC for mongoDB (using gp2 storage class)
 
 - Verify that the storage class is set as default:
  ```sh
  kubectl get storageclass
  ```

 - Create the PVC
  ```sh
  kubectl create -f volume-pvc.yaml
  ```

 - Verify created pvc: 
  ```sh
  kubectl get pv
  ```

2. Create mongoDB
```sh
kubectl apply -f mongo-stack.yaml
```

3. Apply services
```sh
kubectl apply -f services.yaml
```

4. Apply workloads
```sh
kubectl apply -f workloads.yaml
```

5. Verify that pods are runnning
```sh
kubectl get all
```





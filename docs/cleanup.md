

# Deleting the Fleetman application and the EKS Cluster


### Delete pods

1. Delete workloads
```sh
kubectl delete -f workloads.yaml
```

2.Delete services
```sh
kubectl delete -f services.yaml
```

3. Delete mongoDB
```sh
kubectl delete -f mongo-stack.yaml
```

4. Verify that pods,services,and deployments are all deleted
```sh
kubectl get all
```  

5. Delete PVC for mongoDB (using gp2 storage class)
 
 - Delete the PVC
  ```sh
  kubectl delete -f volume-pvc.yaml
  ```

 - Verify if pvc is deleted: 
  ```sh
  kubectl get pv
  ```

  
###  DELETE THE EKSCTL CLUSTER
Delete the cluster when you no longer need it for this workshop

```sh
eksctl delete cluster --name=eks-workshop-eksctl
```










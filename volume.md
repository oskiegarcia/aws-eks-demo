
# Pesistent volume in EBS with gp2 storage class

To create an AWS storage class for your Amazon EKS cluster

1. Create the storage class
```sh
kubectl create -f gp2-storage-class.yaml
```


To define a default storage class

1. List existing storage classes
```sh
kubectl get storageclass
```

2. Choose a storage class and set it as your default by setting the storageclass.kubernetes.io/is-default-class=true annotation.
```sh
kubectl patch storageclass gp2 -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

3. Verify that the storage class is now set as default.
```sh
kubectl get storageclass
```



To create the PVC using the gp2 storage class

```sh
kubectl create -f volume-pvc.yaml
```




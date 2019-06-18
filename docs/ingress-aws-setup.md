
# Setting up NGINX Ingress Controller in AWS 

Reference:
https://kubernetes.github.io/ingress-nginx/deploy/#aws


## Steps

1. deploy mandatory yaml
```sh
kubectl apply -f mandatory.yaml
```

Verify running pods in the new namespace
```sh
kubectl get all --namespace ingress-nginx
```


2. Setup Elastic Load Balancer (Layer 4)
```sh
kubectl apply -f service-l4.yaml
```

Verify running service in the new namespace
```sh
kubectl get all --namespace ingress-nginx
```

```sh
curl http://<DNS of the new Load Balancer>
```

3. Apply patch
```sh
kubectl apply -f patch-configmap-l4.yaml
```

Verify config map values
```sh
kubectl get cm --namespace ingress-nginx
kubectl describe cm nginx-configuration --namespace ingress-nginx
```









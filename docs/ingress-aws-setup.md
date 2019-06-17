
# Setting up NGINX Ingress Controller in AWS 

Reference:
https://kubernetes.github.io/ingress-nginx/deploy/#aws


## Steps

1. deploy mandatory yaml
```sh
kubectl apply -f mandatory.yaml
```

2. Setup Elastic Load Balancer (Layer 4)
```sh
kubectl apply -f service-l4.yaml
```

3. Apply patch
```sh
kubectl apply -f patch-configmap-l4.yaml
```







# Setting up NGINX Ingress Controller in AWS 

References:
https://support.rackspace.com/how-to/modify-your-hosts-file/


## Steps

1. Get IP address of your load balancer
```sh
nslookup <DNS of Load Balancer>
```

2. Edit hosts file to add the following lines
<IP of Load Balancer>  myfleetman.com




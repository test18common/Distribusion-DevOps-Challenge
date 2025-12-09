# Deploying Monitoring Stack with Helm


## 2. Install metalb-lb

Add the fluent-bit Helm chart repository:
```sh
kubectl apply -f metallb-native.yaml
kubectl apply -f metal-lb.yml
```



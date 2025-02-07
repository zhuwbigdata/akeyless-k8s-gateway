# Install Helm Chart
Reference
()

## Create name space for Gateway
```
kubectl create ns wzhu-akeyless
```

## Install / Upgrade 
See helm directory for values.

```
helm repo add akeyless https://akeylesslabs.github.io/helm-charts
helm repo update
helm show values akeyless/akeyless-gateway > values.yaml
helm upgrade --install wzhu-gw akeyless/akeyless-gateway -f values.yaml -n waynez --create-namespace
```

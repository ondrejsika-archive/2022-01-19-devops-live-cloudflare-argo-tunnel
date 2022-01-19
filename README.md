# DevOps Live #28: Cloudflare Argo Tunnel

## Kubernetes

## Setup HELM

```
helm repo add sikalabs https://helm.sikalabs.io
helm repo update
```

## Run Minikube

```
minikube start
minikube addons enable ingress
```

## Run `minikube tunnel`

```
minikube tunnel
```

## Install Example Apps

```
helm upgrade --install red sikalabs/hello-world \
    --set image=sikalabs/hello-world-server:red \
    --set host=red.127.0.0.1.nip.io
helm upgrade --install green sikalabs/hello-world \
    --set image=sikalabs/hello-world-server:green \
    --set host=green.127.0.0.1.nip.io
helm upgrade --install blue sikalabs/hello-world \
    --set image=sikalabs/hello-world-server:blue \
    --set host=blue.127.0.0.1.nip.io
```

## Create `values/secrets.yml`

```
cp ./values/secrets.EXAMPLES.yml ./values/secrets.yml
vim ./values/secrets.yml
```

## Install Tunnels

```
helm upgrade --install red-tunnel sikalabs/cloudflare-tunnel \
    -f ./values/global.yml \
    -f ./values/secrets.yml \
    -f ./values/red.yml
helm upgrade --install gree-tunnel sikalabs/cloudflare-tunnel \
    -f ./values/global.yml \
    -f ./values/secrets.yml \
    -f ./values/green.yml
helm upgrade --install blue-tunnel sikalabs/cloudflare-tunnel \
    -f ./values/global.yml \
    -f ./values/secrets.yml \
    -f ./values/blue.yml
```

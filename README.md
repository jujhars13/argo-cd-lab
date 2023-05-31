# Argo CD lab

![argo logo](logo.webp)

## Getting started

1. Create a minikube cluster
```bash
# create a minikube cluster
# use a VM as opposed to local docker node so we can mess with
# ebpf kernely stuff like cillum
minikube delete

# install kvm
# see https://help.ubuntu.com/community/KVM/Installation
# sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
# sudo adduser $(id -un) libvirt
# virt-host-validate
minikube start --driver=kvm2 --addons registry headlamp ingress
```

```bash
kubectl create namespace argocd

# inspect manifest
curl https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# apply yaml
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# check deployment
kubectl -n argocd get all

# port forward into dashboard service
kubectl -n argocd port-forward svc/argocd-server 8080:443

# get admin panel secret. Username: admin
kubectl -n argocd \
    get secret argocd-initial-admin-secret \
    -o jsonpath="{.data.password}" | \
    base64 -d
```
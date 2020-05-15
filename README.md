# Playing with kubernetes prometheus operator

Documenting from shell history so I remember what i'm doing


Install minikube

```
brew install minikube

minikube start --kubernetes-version=v1.18.1 --memory=6g --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```

Install the k8s operator

```
git clone https://github.com/coreos/prometheus-operator
cd prometheus-operator
git checkout v0.39.0
kubectl apply -f bundle.yaml
```

Apply all the k8s yaml

```
kubectl apply -f manifests
```

Delete the prometheus crd so we can create the secret config - something was
recreating the secret if I deleted it


```
kubectl delete prometheus
```

Create the prometheus secret with config

```
gzip -k config/prometheus.yaml
cd config
kubectl create secret generic prometheus-prometheus --from-file=./prometheus.yaml.gz
```

Reapply prometheus

```
kubectl apply -f manifests/prometheus.yaml
```

Setup a port foward 

```
kubectl port-forward svc/prometheus 9090
```

Now you can see the prometheus admin interface in your browser on localhost:9090



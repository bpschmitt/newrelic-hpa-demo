# newrelic-hpa-demo

### Minikube Settings

```
minikube -p hpa-demo start --extra-config=controller-manager.horizontal-pod-autoscaler-upscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-sync-period=15s --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-stabilization=1m
```

### Create Namespaces

```
kubectl create namespace newrelic-custom-metrics
kubectl create namespace hpa-demo
```

### Create Secrets

```
kubectl create secret generic nrlicensekey -n hpa-demo --from-literal=nrlicensekey=<NR LICENSE KEY> 

kubectl create secret generic newrelic-adapter -n newrelic-custom-metrics --from-literal=account_id=<NR ACCOUNT ID> --from-literal=personal_api_key=<NR USER API KEY>
```

### Apply Manifests

```
kubectl apply -f 01-hello-world-deployment.yaml
kubectl apply -f 02-adapter.yaml
kubectl apply -f 03-externalmetric.yaml
kubectl apply -f 04-hpa.yaml
```

### Port Forward

```
kubectl port-forward svc/hello-world-service -n hpa-demo 8000 &
```

### Generate Load

```
while true; do curl http://localhost:8000/; sleep 0.5; done
```
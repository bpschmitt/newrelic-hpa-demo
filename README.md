# newrelic-hpa-demo

This work is based on the New Relic Custom Metrics adapter found [here](https://github.com/kidk/k8s-newrelic-adapter).


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
kubectl apply -f 02-custom-metrics-adapter.yaml
kubectl apply -f 03-external-metric.yaml
kubectl apply -f 04-hpa.yaml
```

### Verify the Deployment

```
$ kubectl get --raw "/apis/external.metrics.k8s.io/v1beta1" | jq .
```

You should see output similar to this:

```
{
  "kind": "APIResourceList",
  "apiVersion": "v1",
  "groupVersion": "external.metrics.k8s.io/v1beta1",
  "resources": [
  ]
}
```

### Port Forward

```
kubectl port-forward svc/hello-world-service -n hpa-demo 8000 > /dev/null &
```

### Generate Load

```
while true; do curl http://localhost:8000/ > /dev/null; sleep 0.5; done
```
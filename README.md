# newrelic-hpa-demo

### minikube settings

```
minikube -p hpa-demo start --extra-config=controller-manager.horizontal-pod-autoscaler-upscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-sync-period=15s --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-stabilization=1m
```

### create secret

`kubectl create secret generic newrelic -n nr-custom-metrics --from-literal=account_id=<NR ACCOUNT ID> --from-literal=personal_api_key=<NR USER API KEY>`

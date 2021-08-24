# newrelic-hpa-demo

## minikube settings

```
minikube -p hpa-demo start --extra-config=controller-manager.horizontal-pod-autoscaler-upscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-delay=1m --extra-config=controller-manager.horizontal-pod-autoscaler-sync-period=15s --extra-config=controller-manager.horizontal-pod-autoscaler-downscale-stabilization=1m
```

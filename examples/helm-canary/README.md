# Argo Rollouts and Helm

Argo Rollouts will respond to changes in Rollout resources
regardless of the event source. If you package your manifest
with the Helm package manager you can perform Progressive Delivery deployments with Helm

1. Install the Argo Rollouts controller in your cluster: https://github.com/argoproj/argo-rollouts#installation
2. Install the `helm` executable locally: https://helm.sh/docs/intro/install/

## Deploying the initial version

To deploy the first version of your application:

```
git clone https://github.com/urvashisingh/deployment-strategies
cd deployment-strategies/examples
helm install --create-namespace -n <namespace> example ./helm-canary/  --set image.tag=yellow
```

## Perform the second deployment

To deploy the updated version using Canary strategy:

```
helm upgrade --create-namespace  --install -n <namespace> example ./helm-canary/  --set image.tag=blue
```

```
kubectl-argo-rollouts get rollout example-canary-demo -n <namespace>
```

## Promoting the rollout

To advance the rollout and make the new version stable

```
kubectl-argo-rollouts promote example-canary-demo -n <namespace>
```

This promotes container image `argoproj/rollouts-demo` to `blue` status and `Rollout` deletes old replica which runs `argoproj/rollouts-demo:yellow`.

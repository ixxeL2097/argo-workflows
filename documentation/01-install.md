# 01 - Installa Argo Workflows
## Kubernetes
### Helm

- https://github.com/argoproj/argo-helm/tree/master/charts/argo-workflows

Add the helm repo and fetch the chart :
```
helm repo add argo https://argoproj.github.io/argo-helm
helm fetch --untar argo/argo-workflows
```

Install it in argo namespace:
```
helm upgrade -i argo-workflows -n argo --create-namespace argo-workflows/ --set workflow.serviceAccount.create=true \
                                                                          --set workflow.serviceAccount.name=argo-workflow \
                                                                          --set controller.metricsConfig.enabled=true \
                                                                          --set controller.serviceMonitor.enabled=true \
                                                                          --set controller.serviceMonitor.additionalLabels.release=prom
```

Get auth token to authenticate in auth token mode :
```
kubectl exec -it deploy/argo-workflows-server -- argo auth token
```

:pushpin: If you are using `k3s` server, you will need to specify `--set workflow.containerRuntimeExecutor=k8sapi` instead of the default `docker` value (https://surenraju.medium.com/setup-local-dev-environment-for-argo-workflows-with-k3s-and-k3d-9089964ebfdc).

### Install CLI

- https://github.com/argoproj/argo-workflows/releases

```
# Download the binary
curl -sLO https://github.com/argoproj/argo-workflows/releases/download/v3.2.4/argo-linux-amd64.gz

# Unzip
gunzip argo-linux-amd64.gz

# Make binary executable
chmod +x argo-linux-amd64

# Move binary to path
mv ./argo-linux-amd64 /usr/local/bin/argo

# Test installation
argo version
```


### Grafana dashboard
The Grafana dashboard is available at ID 13927 
- https://grafana.com/grafana/dashboards/13927

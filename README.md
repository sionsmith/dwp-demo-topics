# dwp-demo-topics
Topics for DWP demo cluster

# Add a second repo to a cluster
The flux bootstrap can only be used once, as it will setup the context for flux to sync with and all of the CRDs, RBAC etc.

In order to link another repo you will need to create a `source` and a `kustomize` and apply that ot the cluster for example.

```
# private repo
flux create source git team-alpha-resouces \
  --url=ssh://git@github.com/sionsmith/dwp-demo-topics \
  --branch=main

# public repo
flux create source git team-alpha-resouces \
    --url=https://github.com/sionsmith/dwp-demo-topics \
    --branch=main
  
```

THEN

```
flux create kustomization team-alpha-resouces \
  --source=GitRepository/team-alpha-resouces \
  --path="./" \
  --prune=true \
  --interval=5m \
  --namespace=flux-system 
```

To check this has been added run:
```
kubectl get kustomizations
---
kubectl get GitRepositories -A
```
You should see that the status for both will be set to true, if there are any issues the status will be false and you can see the log to fix the error.

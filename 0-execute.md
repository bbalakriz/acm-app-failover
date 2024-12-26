```kind delete cluster -n hub
kind delete cluster -n cluster1
kind delete cluster -n cluster2
```

---

```
clusteradm install hub-addon --names application-manager
clusteradm create clusterset app-clusterset
clusteradm clusterset set app-clusterset --clusters cluster1
clusteradm clusterset set app-clusterset --clusters cluster2
```

---

```
cd /Users/bbalasub/Desktop/delete/jobs-acm-placement
k create namespace application
k config use-context kind-cluster1
k create namespace application

k config use-context kind-cluster2
k create namespace application

k config use-context kind-hub
k apply -f managedclustersetbinding.yaml
k apply -f placement.yaml
```

---

```
clusteradm get placements -otable
k get placementdecision -n application

clusteradm create work app-manifestwork -f work.yaml --placement application/app-placement
```

# to check the status of the mainfestwork

```
k get manifestwork/app-manifestwork -o yaml -n application
```

---

```
podman stop cluster1-control-plane

k config use-context kind-hub
clusteradm get placements -otable
k get managedclusters
```

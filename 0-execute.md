Clean up and initial setup of OCM and managed clusters

```
kind delete cluster -n hub
kind delete cluster -n cluster1
kind delete cluster -n cluster2

curl -L https://raw.githubusercontent.com/open-cluster-management-io/OCM/main/solutions/setup-dev-environment/local-up.sh | bash
```

---

Application manager addons setup

```
k config use-context kind-hub
clusteradm install hub-addon --names application-manager
kubectl create ns open-cluster-management-agent-addon --context cluster1
kubectl create ns open-cluster-management-agent-addon --context cluster2
clusteradm addon enable --names application-manager --clusters cluster1
clusteradm addon enable --names application-manager --clusters cluster2
```

---

ManagedclusterSet setup

```
# clusteradm create clusterset app-clusterset
# clusteradm clusterset set app-clusterset --clusters cluster1
# clusteradm clusterset set app-clusterset --clusters cluster2
k apply -f 01-namespace.yaml
k apply -f 01-managedclustersetbinding.yaml

```

---

```
k config use-context kind-cluster1
k apply -f rbac.yaml

k config use-context kind-cluster2
k apply -f rbac.yaml

k config use-context kind-hub
k apply -f 01-channel.yaml
k apply -f 02-placement.yaml
k apply -f 02-subscription.yaml
```

---

```
clusteradm get placements -otable
k get placementdecision -n application

```

---

Check the status of the mainfestwork

```
k get manifestwork -A
```

---

Stop cluster1 and check the failover

```
podman stop cluster1-control-plane

k config use-context kind-hub
clusteradm get placements -otable
k get managedclusters
```

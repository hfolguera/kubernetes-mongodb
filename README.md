# kubernetes-mongodb
Repository to deploy MongoDB to local Kubernetes cluster. In this example, bitnami Helm chart is used to deploy the instances.

## Installation
### 1. Configure MongoDB HELM repo
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### 2. Verify Jenkins HELM repo
```
helm search repo bitnami
```

### 3. Create a Namespace for Jenkins
```
kubectl apply -f mongodb-namespace.yaml
```

### 4. Create Persistent Volume
```
kubectl apply -f mongodb-standalone-volume.yaml
```
or
```
kubectl apply -f mongodb-replicaset-volume.yaml
```

Since we are using 3-node k8s cluster, we need to use a NFS storage class instead of hostPath.

Persistent Volume is set to use NFS volume. The directory must exist with mongodb owner as user and group (1001:1001). 
Connect to your NFS server, in my case a Synology NAS, and execute the following:
```
sudo chown -R 1001:1001 /volume2/NFS/homelab/mongodb-volume
sudo chmod -R 777 /volume2/NFS/homelab/mongodb-volume
```

### 6. Define installation configuration
Modify configuration by editing `values-standalone.yaml` or `values-replicaset.yaml` file or use the existing one. It has been set to use metallb loadbalancer

### 7. Install MongoDB Chart
```
helm install mongodb-standalone -n mongodb -f values-standalone.yaml bitnami/mongodb
```
or
```
helm install mongodb-replicaset -n mongodb -f values-replicaset.yaml bitnami/mongodb
```

## References
https://github.com/bitnami/charts/tree/master/bitnami/mongodb/#installing-the-chart

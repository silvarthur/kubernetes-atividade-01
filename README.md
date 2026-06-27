# kubernetes-atividade-01

kubectl get pods
kubectl get deployments

kubectl describe TYPE NAME_PREFIX
TYPE: type of the object, "pods" for example.
NAME_PREFIX: the prefix of the name of an object.

## Using kind to Create the Cluster

### Creating the Cluster
kind create cluster --name kubernetes-atividade-01

### Was the Cluster Created?
kind get clusters

### To Interact With the Cluster
kubectl cluster-info --context kind-kubernetes-atividade-01

### To Delete the Cluster:
kind delete kubernetes-atividade-01
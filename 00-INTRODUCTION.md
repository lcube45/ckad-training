# CKA/CKAD certification documentation

## Introduction

https://www.cncf.io/training/certification/ckad/

https://github.com/cncf/curriculum

https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2

## Summary

- [Core concepts](./01-CORE-CONCEPTS.md)
- [Configuration](./02-CONFIGURATION.md)
- [Multi-Container Pod](./03-MULTI-CONTAINER-PODS.md)
- [Observability](./04-OBSERVABILITY.md)
- [Pod Design](./05-POD-DESIGN.md)
- [Services & Networking](./06-SERVICES-NETWORKING.md)
- [Volumes](./07-VOLUMES.md)

## Exercies

https://github.com/dgkanatsios/CKAD-exercises

https://thospfuller.com/2020/11/09/answers-to-five-kubernetes-ckad-practice-questions-2021/

## Useful snippets

```sh
# create a pod in an imperative way
kubectl run nginx --image=nginx

# create a pod in a declarative way
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-definition.yaml
kubectl apply -f pod-definition.yaml

# create a pod and expose it
kubectl run nginx --image=nginx --port=80 --expose

# create a pod and override arguments
kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>

# create a pod and override command and arguments
kubectl run nginx --image=nginx --command -- <cmd> <arg1> <arg2> ... <argN>

# describe an object
kubectl describe <object-type> <object-name>

# edit an object
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
kubectl apply -f pod-definition.yaml

# edit an object (alternative)
kubectl edit pod <pod-name>
kubectl replace --force -f /tmp/kubectl-edit-xxx.yaml

# get documentation on a primitive
kubectl explain <object-type>

# scale a replicaset in an imperative way
kubectl scale replicaset <rs-name> --replicas=10

# get an overview of objects
kubectl get all

# switch temporary to an another namespace
kubectl config set-context $(kubectl config current-context) --namespace=dev

# create a service
kubectl expose pod redis --port=6379 --name=redis-services --dry-run=client -o yaml > service-definition.yaml
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml > service-definition.yaml

# check admission plugins status
kube-apiserver -h | grep enable-admission-plugins

# check api resources
kubectl api-resources --namespaced - o name > file.txt

# find the fields for supported resources
kubectl explain pod.spec.nodeSelector
kubectl explain pod.spec | grep -i nodeselector

# explain specific matters
kubectl explain pod.spec.securityContext
kubectl explain pod.spec.containers.securityContext

# filter documentation
kubectl explain pod --help | more
kubectl explain pod --help | head -30

```
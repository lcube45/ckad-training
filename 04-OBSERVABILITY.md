# CKA/CKAD certification documentation

## Observability

Pod phases :

- Pending
- ContainerCreating
- Running
- Succeeded
- Failed

https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/

### Readiness probes

Sometimes, applications are temporarily unable to serve traffic. For example, an application might need to load large data or configuration files during startup, or depend on external services after startup. A pod with containers reporting that they are not ready does not receive traffic through Kubernetes Services.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    name: simple-webapp
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
      - containerPort: 8080
    readinessProbe:
      httpGet:
        path: /api/ready
        port: 8080
```

```yaml
readinessProbe:
  httpGet:
    path: /api/ready
    port: 8080
  initialDelaySeconds: 10 #warm up delay
  periodSeconds: 5 #check readiness every x seconds
  failureThreshold: 8 #attemps nb
```

```yaml
readinessProbe:
  tcpSocket:
    port: 3306
```

```yaml
readinessProbe:
  exec:
    command:
      - cat
      - /app/is_ready
```

### Liveness probes

A liveness probe can be configured on the container to periodically test whether the application within the container is actually healthy.

```yaml
livenessProbe:
  httpGet:
    path: /api/ready
    port: 8080
```

### Container logging

```sh
kubectl logs <pod-name> <container-name>
```

### Monitor and debug

The metrics server retrieves metrics from each of the kubernetes nodes and pods, aggregates them and stores them in memory. Note that the metrics server is only an 
in-memory monitoring solution and does not store the metrics on the disk, and as a result you cannot see historical performance data. 

Kubernetes runs an agent on each node known as the **kubelet**, which is responsible for receiving 
instructions from the kubernetes API master server and running PODs on the nodes. The kubelet also contains a subcomponent known as as **cAdvisor or Container 
Advisor**. cAdvisor is responsible for retrieving performance metrics from pods, and exposing them through the kubelet API to make the metrics available for the Metrics Server.

https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

```sh
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl apply -f ./metrics-server/
```

```sh
kubectl top nodes
kubectl top pods
```

https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/
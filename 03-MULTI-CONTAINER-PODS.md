# CKA/CKAD certification documentation

## Multi-container pods

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello
spec:
  containers:
  - name: hello
    image: busybox
  - name: log-agent
    image: log-agent
```

### Design patterns

- Sidecar
  - Example: deploying à logging agent along side a web server
- Adapter
  - Example: a adapter container to process the logs before sending them
- Ambassador
  - Example : when you want to connect to database dependenting the environment, deploy a pod who acts as a proxy

### Init Containers

That is a task that will be run only one time when the pod is first created. Or a process that waits for an external service or database to be up before the actual application starts.

When a POD is first created the initContainer is run, and the process in the initContainer must run to a completion before the real container hosting the application starts.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```
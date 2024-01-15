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
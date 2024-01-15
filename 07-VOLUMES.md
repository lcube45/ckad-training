# CKA/CKAD certification documentation

## Volumes

Docker containers are meant to be transient in nature. Which means they are meant to last only for a short period of time. They are called upon when required to process data and destroyed once finished. The same is true for the data within the container. The data is destroyed along with the container.

To persist data processed by the containers, we attach a volume to the containers when they are created. The data processed by the container is now placed in this 
volume, thereby retaining it permanently. Even if the container is deleted, the data generated or processed by it remains.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
spec:
  containers:
  - image: alpine
    name: alpine
    ommand: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/numbers.out;"]
    volumeMounts:
    - mountPath: /opt
      name: data-volume
  
  volumes:
  - name: data-volume
    hostpath:
      path: /data
      type: Directory
```

## Persistent Volumes

A Persistent Volume is a Cluster wide pool of storage volumes configured by an Administrator, to be used by users deploying applications on the cluster. The users can now select storage from this pool using Persistent Volume Claims.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: slow
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /tmp
    server: 172.17.0.2
```

### Persistent Volume Claims

Persistent Volumes and Persistent Volume Claims are two separate objects in the Kubernetes namespace. An Administrator creates a set of Persistent Volumes and a 
user creates Persistent Volume Claims to use the storage. **Once the Persistent Volume Claims are created, Kubernetes binds the Persistent Volumes to Claims based on the request and properties set on the volume.**

There is a one-to-one relationship between Claims and Volumes, so no other claim can utilize the remaining capacity in 
the volume.

If there are no volumes available the Persistent Volume Claim will remain in a pending state, until newer volumes are made available to the cluster. Once newer 
volumes are available the claim would automatically be bound to the newly available volume.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: foo-pvc
  namespace: foo
spec:
  storageClassName: "" # Empty string must be explicitly set otherwise default StorageClass will be set
  volumeName: foo-pv
  ...
```
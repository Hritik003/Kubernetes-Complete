# Managing Data and volumes with kubernetes

## `Normal` Volumes vs `Persistent` Volumes

- Volumes allow you to persist data be it `normal` volume and `persistent` volume
- When it comes to normal volume, it is imp to realize it is dependent of pod survival, it is attached to the pod lifecycle.
- `Normal` Volume are defined and created with the pod and your data is lost when your volume is removed.
- Repetative and hard to administer on a global level.

--- 

  and this is the problem `persistent` volume solve,
- this is a standalone cluster resource( NOT attached to a pods).
- Created standalone, claimed via a PVC.
- Can be defined once and used multiple times.

  so thats the difference, there is no better or best volume here. IT solely depends on the project you are working on.

## Creating persistent volume

Just like we created a deployment.yaml file, we do create a .yaml file for creating a persistent volume,

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: host-pv
spec: 
  capacity: 
    storage: 1Gi
  volumeMode: Filesystem
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  hostPath: 
    path: /data
    type: DirectoryOrCreate
```

and thus a persistent volume gets created in the cluster on executing `kubectl apply -f=host-pv.yaml `. But to access this **PV** from the cluster from inside the pod, we again have to create persistent volume claim `.yaml` file to claim it from the cluster.

``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: host-pvc
spec:
  volumeName: host-pv
  accessModes:
    - ReadWriteOnce
  StorageClassName: standard
  resources:
    requests: 
      storage: 1Gi
```

and also in the deployment.yaml, file we need to add this snippet,

```yaml
 . . . .
      volumes:
        - name: story-volume
          persistentVolumeClaim: 
            claimName: host-pvc
```


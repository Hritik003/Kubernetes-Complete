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


## Summary 

Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) are crucial components in Kubernetes for managing storage resources. Here's an overview:

### **Persistent Volume (PV):**
- **Definition**: A Persistent Volume is a piece of storage in the cluster that has been provisioned by an administrator or dynamically through storage classes. It is a resource in the cluster just like a node is a cluster resource.
  
- **Lifecycle**: A PV has an independent lifecycle separate from any individual Pod that uses the PV. Once provisioned, it remains available until it is explicitly deleted by the cluster administrator or through specific reclaim policies.

- **Types of Storage**: PVs can represent various storage types, such as NFS shares, iSCSI disks, cloud storage (like AWS EBS or GCE PD), or even hostPath (a path on the node's filesystem).

- **VolumeMode**: 
  - **Filesystem**: The storage is mounted as a directory inside the Pod.
  - **Block**: The storage is mounted as a raw block device inside the Pod.

- **Access Modes**:
  - **ReadWriteOnce (RWO)**: The volume can be mounted as read-write by a single node.
  - **ReadOnlyMany (ROX)**: The volume can be mounted read-only by many nodes.
  - **ReadWriteMany (RWX)**: The volume can be mounted as read-write by many nodes.

- **Reclaim Policies**:
  - **Retain**: The PV is not deleted when the associated PVC is deleted, allowing for manual reclamation or further use.
  - **Delete**: The PV and its associated storage are deleted.
  - **Recycle**: The PV is wiped clean (e.g., `rm -rf /thevolume/*`) and made available again for a new PVC.

### **Persistent Volume Claim (PVC):**
- **Definition**: A Persistent Volume Claim is a request for storage by a user. It is similar to a Pod requesting specific resources (like CPU or memory). A PVC specifies size, access modes, and an optional storage class.

- **Binding**: When a PVC is created, Kubernetes searches for an available PV that matches the requested resources and binds the PVC to that PV. Once bound, the PVC exclusively uses that PV.

- **Dynamic Provisioning**: If no suitable PV is available, and the PVC specifies a `storageClassName`, Kubernetes can automatically create a PV on demand using a StorageClass, which defines the parameters for storage creation (e.g., type of disk, performance characteristics).

- **Access Modes**: Like PVs, PVCs specify access modes (RWO, ROX, RWX) which the PV must match.

### **How They Work Together:**
1. **Provisioning**:
   - **Static**: An administrator manually creates PVs. Users then create PVCs, which will be bound to a suitable PV.
   - **Dynamic**: Users create PVCs specifying a StorageClass, and the PV is dynamically created and bound to the PVC.

2. **Usage**: Pods reference PVCs in their volume specifications. The PVC provides the necessary storage to the Pod, abstracting the underlying storage details.

3. **Management**: PVs and PVCs are managed through Kubernetes APIs. Administrators can monitor and control their lifecycle, and reclaim storage when no longer needed.

In this application:
- The `PersistentVolume` named `host-pv` is created with 1Gi of storage.
- The `PersistentVolumeClaim` named `host-pvc` requests 1Gi of storage.
- The `Pod` named `story` uses the `host-pvc` to mount the storage at `/data` inside the container.

This setup ensures that your application data is persisted beyond the lifecycle of individual Pods.

---

This was another great thing that we learnt about `Kubernetes Persistent Volumes`. See you in the other module on Networking


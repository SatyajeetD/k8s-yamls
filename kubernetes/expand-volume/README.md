#  Volume expansion
Paviliondata `pvl-csi-plugin` supports both **online** and **offline** volume expansion. Following examples shows how to do **online** and **offline** volume expansion.
  
```bash
[root@node1 csi-pavilion]# pwd
/pavilion/pvl-csi-plugin/src/csi-pavilion/
[root@node1 csi-pavilion]# cd ./examples/kubernetes/expand-volume/
```
  

### Example 1 : Volume is OFFLINE  [Unassigned]

Volume is not attached to **POD**.

### Create Volume
```bash
[root@node1 expand-volume]# kubectl create -f pvc.yaml
persistentvolumeclaim/csi-expand-pvc created
[root@node1 expand-volume]# kubectl get pvc -o wide
NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE   VOLUMEMODE
csi-expand-pvc   Bound    pvc-5cf27ed1-47fc-4e23-a89d-d619d82ad070   100Gi      RWO            pvl-nvme-storage   66s   Filesystem
```

  

### Expand Volume

```bash
[root@node1 expand-volume]# kubectl edit pvc csi-expand-pvc
```
Do changes in storage size as per below example. **Save** and **Exit** the file.-
```bash
"""
"""
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
--    storage: 100Gi
++    storage: 200Gi

"""
"""
```
```bash
persistentvolumeclaim/csi-expand-pvc edited
[root@node1 expand-volume]# kubectl get pvc -o wide
NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE     VOLUMEMODE
csi-expand-pvc   Bound    pvc-5cf27ed1-47fc-4e23-a89d-d619d82ad070   200Gi      RWO            pvl-nvme-storage   3m25s   Filesystem
 ```
### Example 2 : volume is ONLINE [Online] 

Volume is attached to **POD**.

  

### Create Volume
```bash
[root@node1 expand-volume]# kubectl create -f pvc.yaml
```
  

### Attach Volume
```bash
[root@node1 expand-volume]# kubectl create -f pod.yaml
```
  

### Expand Volume
```bash
[root@node1 expand-volume]# kubectl edit pvc csi-expand-pvc
```
Do changes in storage size as per below example. **Save** and **Exit** the file.-
```bash
"""
"""
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
--    storage: 100Gi
++    storage: 200Gi

"""
"""
```

## Note

### Expand volume -
There is another way to expand volume by configuring pvc and applying it.

**Step 1** - Create PersistentVolumeClaim by using following command -
```bash
[root@node1 expand-volume]# kubectl apply -f pvc.yaml
persistentvolumeclaim/csi-expand-pvc created
```

**Step 2**- Edit pvc by using your favorite editor. ( I am using vim).
```bash
[root@node1 expand-volume]# vim pvc.yaml

"""
"""
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
--    storage: 100Gi
++    storage: 200Gi

"""
"""
```
**Step 3** - Apply edited pvc.
```bash
[root@node1 expand-volume]# kubectl apply -f pvc.yaml
persistentvolumeclaim/csi-expand-pvc configured
```
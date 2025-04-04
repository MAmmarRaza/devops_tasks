![image](https://github.com/user-attachments/assets/d2292b8b-c2cf-487e-bf22-d29aaa59106e)


![image](https://github.com/user-attachments/assets/d2be735b-e5ba-437f-a83c-0c599940667e)

# PV
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nginx-pv
spec:
  capacity:
    storage: 1Gi   # Storage size
  accessModes:
    - ReadWriteOnce  # Only one node can write at a time
  persistentVolumeReclaimPolicy: Retain  # Keeps data even if PVC is deleted
  storageClassName: manual
  hostPath:
    path: "/mnt/data"  # Directory on worker node
```

# PVC
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi  # Requesting 500MB of storage
  storageClassName: manual
```

# Pod definition
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: nginx-storage
          mountPath: /var/log/nginx  # Where logs will be stored inside the Pod
  volumes:
    - name: nginx-storage
      persistentVolumeClaim:
        claimName: nginx-pvc  # This binds the PVC to the Pod
```


📌 Key Learnings
- ✅ PV = Physical Storage (provided by admin)
- ✅ PVC = User Request (claims storage from PV)
- ✅ Data is Persistent → Remains after Pod restarts


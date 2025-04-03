# Cluster Ip
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  selector:
    app: nginx  # Matches Deployment labels
  ports:
    - protocol: TCP
      port: 80         # Service port (Cluster-wide)
      targetPort: 80   # Container port
  type: ClusterIP
```

# Node Port
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport
spec:
  selector:
    app: nginx  # Matches Deployment labels
  ports:
    - protocol: TCP
      port: 80         # Service port (Cluster-wide)
      targetPort: 80   # Container port
      nodePort: 30080  # Exposes service on each Node's IP at port 30080
  type: NodePort
```

# Load Balancer
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-loadbalancer
spec:
  selector:
    app: nginx  # Matches Pods with label app=nginx
  ports:
    - protocol: TCP
      port: 80        # The port exposed by the LoadBalancer
      targetPort: 80  # The container's port inside the Pod
  type: LoadBalancer
```

## Simple POD

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    env: dev
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
      resources:
        requests:
          cpu: "250m"
          memory: "256Mi"
        limits:
          cpu: "250m"
          memory: "256Mi"
      env:
        - name: APP_ENV
          value: production
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef:
              name: nginx-config
              value: LOG_LEVEL
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: nginx-secret
              value: DB_PASSWORD
      livenessProd:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 5
      readinessProd:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 5

```




## ConfigMaps
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "debug"
```

## Secrets
```
apiVersion: v1
kind: Secret
metadata:
  name: nginx-secret
type: Opaque
data:
  DB_PASSWORD: dXNlcjEyMw==  # Base64 encoded value of "user123"
  API_KEY: ZGVtb2tleQ==  # Base64 encoded value of "demokey"
```










Perfect! Here's a full working setup for you:

---

## 🚀 1️⃣ Node.js App with Prometheus Metrics

### `server.js`
```javascript
const express = require('express');
const promClient = require('prom-client');

const app = express();
const collectDefaultMetrics = promClient.collectDefaultMetrics;
collectDefaultMetrics();

const httpRequestCounter = new promClient.Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
});

app.get('/', (req, res) => {
  httpRequestCounter.inc();
  res.send('Hello from Node.js!');
});

app.get('/metrics', async (req, res) => {
  res.set('Content-Type', promClient.register.contentType);
  res.end(await promClient.register.metrics());
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`App listening on port ${PORT}`));
```

---

## 🚀 2️⃣ Dockerfile
```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

> **Add `package.json`:**
```json
{
  "name": "nodejs-metrics-app",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2",
    "prom-client": "^14.1.1"
  }
}
```

---

## 🚀 3️⃣ Kubernetes Deployment YAML (`nodejs-app-deployment.yaml`)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs-app
  labels:
    app: nodejs-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nodejs-app
  template:
    metadata:
      labels:
        app: nodejs-app
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/path: "/metrics"
        prometheus.io/port: "3000"
    spec:
      containers:
      - name: nodejs-app
        image: <YOUR_ECR_IMAGE>
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: nodejs-app
  labels:
    app: nodejs-app
spec:
  ports:
  - port: 80
    targetPort: 3000
    protocol: TCP
  selector:
    app: nodejs-app
```

> ⚠️ Replace `<YOUR_ECR_IMAGE>` with your image from Amazon ECR.

---

## 🚀 4️⃣ ServiceMonitor YAML (`nodejs-app-servicemonitor.yaml`)
```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: nodejs-app-monitor
  labels:
    release: prometheus
spec:
  selector:
    matchLabels:
      app: nodejs-app
  endpoints:
  - port: http
    path: /metrics
    interval: 15s
```

---

## 🚀 5️⃣ Deploy everything:
```bash
kubectl apply -f nodejs-app-deployment.yaml
kubectl apply -f nodejs-app-servicemonitor.yaml
```

---

## ✅ 6️⃣ Verify:
- Check pods:
  ```bash
  kubectl get pods
  ```
- Check ServiceMonitor:
  ```bash
  kubectl get servicemonitors
  ```

---

## ✅ 7️⃣ Grafana Dashboard:
- Log into Grafana (`http://localhost:3000`).
- Add Prometheus data source (should already exist with kube-prometheus-stack).
- Import Node.js dashboards from [Grafana Dashboards](https://grafana.com/grafana/dashboards/).

---

Would you like me to:
✅ Bundle this as a GitHub-ready repo?  
✅ Provide the `kubectl` and `helm` full install commands in one script?  
✅ Help with ECR image build and push commands?

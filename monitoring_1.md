Awesome! Here's a clear **step-by-step guide** to deploy **Prometheus + Grafana on EKS** and monitor a **Node.js app** using **Node Exporter**.

---

## ✅ Full Step-by-Step Guide:

---

### 1️⃣ **Prepare your environment:**
- ✅ AWS CLI installed and configured (`aws configure`)
- ✅ `eksctl` installed
- ✅ `kubectl` installed
- ✅ `helm` installed (`Helm` is needed to deploy Prometheus and Grafana easily)

Install Helm if missing:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

### 2️⃣ **Create an EKS cluster** (simple example):
```bash
eksctl create cluster \
  --name monitoring-cluster \
  --version 1.27 \
  --region us-east-1 \
  --nodes 2 \
  --node-type t3.medium
```

---

### 3️⃣ **Install Prometheus and Grafana via Helm:**

#### Add the Helm repo:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

#### Install Prometheus:
```bash
helm install prometheus prometheus-community/kube-prometheus-stack
```

#### Verify it's running:
```bash
kubectl get pods -n default
```

---

### 4️⃣ **Access Grafana:**

Find Grafana service:
```bash
kubectl get svc
```
Look for something like `prometheus-grafana` and forward the port:
```bash
kubectl port-forward svc/prometheus-grafana 3000:80
```

Now open `http://localhost:3000`

> **Default login:**
> - Username: `admin`
> - Password: `prom-operator` (you can check the password via `kubectl get secret prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode`)

---

### 5️⃣ **Deploy Node.js app (example):**

Here's a simple Node.js app with metrics:

#### Example `server.js`:
```javascript
const express = require('express');
const promClient = require('prom-client');

const app = express();
const collectDefaultMetrics = promClient.collectDefaultMetrics;
collectDefaultMetrics();

app.get('/', (req, res) => res.send('Hello from Node.js!'));
app.get('/metrics', async (req, res) => {
  res.set('Content-Type', promClient.register.contentType);
  res.end(await promClient.register.metrics());
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`App listening on port ${PORT}`));
```

#### Dockerize and push to ECR:
- Create `Dockerfile`
- Build image
- Push to ECR
- Deploy to EKS (I can give you the full YAML for deployment if you'd like)

---

### 6️⃣ **Install Node Exporter (for node-level metrics):**
```bash
helm install node-exporter prometheus-community/prometheus-node-exporter
```

---

### 7️⃣ **Configure Prometheus to scrape your Node.js app:**
You need to add a **ServiceMonitor** or **Pod annotations** to tell Prometheus where your app's `/metrics` endpoint is.

Example ServiceMonitor YAML:
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

### 8️⃣ **Check metrics in Grafana:**
- Grafana will already have Prometheus as a data source.
- Import dashboards for Node Exporter and your Node.js app.
- You can find ready-made dashboards at [Grafana Dashboards](https://grafana.com/grafana/dashboards/).

---

### ✅ Optional:
I can provide:
- Full Node.js app with Docker and Kubernetes deployment YAML.
- ServiceMonitor YAML.
- Example dashboards to import.

Want me to build out these files for you?

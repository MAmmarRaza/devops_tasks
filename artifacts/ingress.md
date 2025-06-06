## Load Balancer Configuration
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-eks-newblog
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/ssl-redirect: '443'
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:905418218028:certificate/aafea59c-50c0-4163-b8b3-d261a8697f5f
spec:
  ingressClassName: alb
  rules:
  - host: raza.exportthreads.live
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: newblog-service
            port:
              number: 5001
```
## another configuration
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
  generation: 3
  name: ingress-pay
  namespace: critical-space
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: pay-service
            port:
              number: 8282
        path: /pay
        pathType: Prefix
```

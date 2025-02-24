# Need Domain First

Note: As i have domain on cloudflare 

1- Create route 53 record for cloud.maxes.live

2- Attach ACM for this domain cloud.maxes.live

3- sign in with credentials on cloudflare
```
ammarraza494@gmail.com
```
4- Go to DNS setting and add route 53 nameservers for sub domain cloud

5- Create kubernetes cluster after setting secret and id of aws user in terminal 
``eksctl create cluster -f cluster-config.yaml``

```
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: ammarcluster
  region: us-east-1

managedNodeGroups:
  - name: eks-mng
    instanceType: t2.medium
    desiredCapacity: 2

iam:
  withOIDC: true
  serviceAccounts:
  - metadata:
      name: aws-load-balancer-controller
      namespace: kube-system
    wellKnownPolicies:
      awsLoadBalancerController: true

addons:
  - name: aws-ebs-csi-driver
    wellKnownPolicies: # Adds an IAM service account
      ebsCSIController: true
      
cloudWatch:
 clusterLogging:
   enableTypes: ["*"]
   logRetentionInDays: 30
```

6- Run these commands
```
eksctl update addon --name vpc-cni --cluster ammarcluster

eksctl utils migrate-to-pod-identity --cluster web-quickstart

kubectl get serviceaccount aws-load-balancer-controller -n kube-system -o yaml | grep eks.amazonaws.com/role-arn

helm repo add eks https://aws.github.io/eks-charts
helm repo update

 helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=web-quickstart \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```
7- Add argo cd 
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install argo-cd argo/argo-cd -n argocd --create-namespace

```

```
kubectl patch svc argo-cd-argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

```
kubectl get pods -n argocd
```
8- Apply ingress file ``kubectl apply -f argocd-ingress.yaml``
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: argocd-ingress
  namespace: argocd
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/group.name: argocd
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:851725392626:certificate/b4d85617-25f3-4438-9fe4-29a3477dcfe0
    kubernetes.io/ingress.class: alb
spec:
  rules:
    - host: cloud.maxes.live
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: argo-cd-argocd-server
                port:
                  number: 80
```

9- Check ingress
```
kubectl get ingress argocd-ingress -n argocd
```
10- Attach alb to domain 
![image](https://github.com/user-attachments/assets/1c16352f-7499-43da-ba12-8d268b7a4d08)

11- if still not run these commands

```
kubectl get configmap argocd-cmd-params-cm -n argocd -o yaml
```
Look for:
``server.insecure: "true"``

- if not true then
```
kubectl patch configmap argocd-cmd-params-cm -n argocd --type merge -p '{"data":{"server.insecure":"true"}}'
kubectl rollout restart deployment argocd-server -n argocd
```

12- Remove cache and cookies and try to run https://cloud.maxes.live

13- by default username is ``admin`` while password can be get through this command
```
kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode
```











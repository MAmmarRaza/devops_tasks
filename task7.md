## Kubernetes Cluster With EKS

1- Configure and Install AWS cli
2- Install EKS
```bash
curl -s --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
3- Kubectl
```bash
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```
```bash
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```bash
sudo apt-get update
sudo apt-get install -y kubectl
sudo snap install kubectl --classic
kubectl version --client
```

## Creating Cluster

```bash
#cluser.yaml file
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

```bash
eksctl create cluster -f cluster-config.yaml
kubectl create deploymen nginx-depl --image=nginx
kubectl get replicacset
kubetctl get deployment
kubectl get pod
kubectl get service 
kubectl get all

kubectl edit deployment nginx-depl

kubectl exec -it mong-container -- bin/bash

kubectl delete deployment mongo-deployment
kubectl get nodes -o wide

kubectl get  svc -o wide

 kubectl apply -f abc.yml

 kubectl logs pod-name
kubectl describe service service-name
minikube service mongo-express-service
```

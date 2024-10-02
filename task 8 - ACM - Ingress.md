To create the SSL certificate and get the ACM certificate ARN for your Kubernetes Ingress in AWS, you need to follow these steps:

### Step 1: Request a Certificate in AWS ACM (Amazon Certificate Manager)

1. **Log in to the AWS Management Console** and navigate to the **ACM (Amazon Certificate Manager)** dashboard.

2. **Request a Public Certificate**:
   - In ACM, click on **Request a certificate**.
   - Choose **Request a public certificate** (for public websites).
   - Click **Next**.

3. **Add Domain Names**:
   - In the **Domain name** field, enter the domain you want to secure (e.g., `raza.exportthreads.live`).
   - If you want to cover both the root domain and subdomains, you can add multiple domains, like `raza.exportthreads.live` and `*.raza.exportthreads.live`.
   - Click **Next**.

4. **Select Validation Method**:
   - Choose **DNS validation** or **Email validation**. 
     - **DNS validation** is preferred if you have control over the DNS records.
     - **Email validation** works by sending a verification email to the domain’s admin email address.
   - Follow the instructions for the selected validation method.

5. **Review and Request**:
   - Review the settings and click **Request**.

6. **Complete Validation**:
   - If you chose DNS validation, you’ll need to add a CNAME record to your domain’s DNS settings to validate ownership.
   - Once the DNS record is added or the email is verified, the certificate status in ACM will change to **Issued**.

### Step 2: Get the Certificate ARN

1. **View the Certificate in ACM**:
   - After the certificate is issued, go back to the **ACM dashboard**.
   - Find your certificate and click on it.
   
2. **Copy the ARN**:
   - The **Certificate ARN** will look something like this:  
     `arn:aws:acm:us-east-1:123456789012:certificate/abcde1234-5678-90ab-cdef-EXAMPLE`.
   - Copy this ARN.

### Step 3: Add the Certificate ARN to Your Ingress

Now, update your Kubernetes Ingress with the ARN of your SSL certificate:

```yaml
annotations:
  alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:<region>:<account-id>:certificate/<certificate-id>
```

Replace `<region>`, `<account-id>`, and `<certificate-id>` with the values from the ARN you copied.

Once you've done this, your ALB will use the SSL certificate, and you can serve your application over HTTPS.

### Step 4: Update DNS Records (Optional)
- Ensure that your domain's DNS settings point to the AWS Load Balancer’s public DNS or IP address. You may need to add an A record or CNAME depending on your DNS provider.

After completing these steps, your domain will be secured with HTTPS.


```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-eks-newblog
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS": 443}]'
    alb.ingress.kubernetes.io/ssl-redirect: '443' # Enable redirect from HTTP to HTTPS
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:<region>:<account-id>:certificate/<certificate-id> # Replace with your ACM certificate ARN
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

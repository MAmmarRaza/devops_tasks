# Load Balancer Task

## Create VPC
![createVPC](https://github.com/user-attachments/assets/31a7f848-e0a0-48d8-8653-ff6e407e1fa9)

## Create Security Group

![image](https://github.com/user-attachments/assets/a8a93918-3df5-448f-8b6b-07c0f3bc5e9c)

![image](https://github.com/user-attachments/assets/b7f15dee-c33b-4caa-b383-afebc495d9c6)

## EC2 create target groups
![image](https://github.com/user-attachments/assets/f996827c-6347-4cb2-86ad-ca4b63474c1e)

## Create Load Balancer

![screencapture-us-east-1-console-aws-amazon-ec2-home-2024-09-05-16_42_23](https://github.com/user-attachments/assets/694ecb33-f84c-4c8a-b56a-3db2fa46ec9b)

## Now Launch Template
![image](https://github.com/user-attachments/assets/f1bf8d86-8f74-458b-8fa2-6c8fb45a5172)

![image](https://github.com/user-attachments/assets/0246a4a3-7732-4cd3-a9ac-38bf3b7d0803)

![image](https://github.com/user-attachments/assets/c9f6eccb-415e-49af-9b4b-0ba1c114ef35)

```bash
#!/bin/bash

# Update the system and install Apache
yum update -y
yum install -y httpd

# Start and enable Apache to run on boot
systemctl start httpd
systemctl enable httpd

# Fetch the latest token for accessing EC2 instance metadata
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")

# Fetch the instance ID
INSTANCEID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id -H "X-aws-ec2-metadata-token: $TOKEN")

# Create or overwrite index.html with the instance ID information
echo "<center><h1>This instance has the ID: $INSTANCEID </h1></center>" > /var/www/html/index.html

# Ensure httpd can read the index.html file and its directory
chown apache:apache /var/www/html/index.html
chmod 755 /var/www/html
chmod 644 /var/www/html/index.html

# Restart Apache to apply changes
systemctl restart httpd
```

## Enable your both public subnets enable auto ipv4 assigning in edit option
![image](https://github.com/user-attachments/assets/bf8d8dfa-b65f-48f7-b624-5b3726faae4c)

## Create Autoscaling groups

![image](https://github.com/user-attachments/assets/0eec6fe2-adc8-4884-9b98-bf0fbbe54b32)

![image](https://github.com/user-attachments/assets/4a9933bc-74b9-433b-8eae-90e712b7ae5f)

![image](https://github.com/user-attachments/assets/88862501-b3cb-403b-9ff4-9384b3039374)

![image](https://github.com/user-attachments/assets/9cbc6dee-af59-4603-890c-07039e8fe6fb)

![screencapture-us-east-1-console-aws-amazon-ec2-home-2024-09-05-16_59_52](https://github.com/user-attachments/assets/8603f3b7-d334-49dc-af38-c5287d7a47ea)

## Testing load balancer copy dns name from load balancer and paste then try to refresh browser
![image](https://github.com/user-attachments/assets/2b78ae41-e2db-4adc-a18f-3b2df7e99cb8)

![image](https://github.com/user-attachments/assets/b5f4ebfb-fd03-4e94-9fad-c07932144a33)

## Testing auto scaling
![image](https://github.com/user-attachments/assets/a8059c73-6f1b-40c3-b635-e904015a9ac5)

## Alarm on high load
![image](https://github.com/user-attachments/assets/214f0bde-5ecf-49a1-ac70-14b4f0e098a5)




## Check health status of target groups
![image](https://github.com/user-attachments/assets/be2ee9de-8ed5-41dc-b139-7543f978a055)

## troubleshooting if unhealthy
1- give security groups to anywhere http


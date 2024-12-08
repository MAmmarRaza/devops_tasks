![image (3)](https://github.com/user-attachments/assets/07af1213-e1c4-4d34-b297-7fecd5c6e3e4)

## Deploying Frontend on S3 and backend on EC2 connected to RDS Database

1- Configure AWS CLI

```bash
sudo apt install unzip curl -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
```

2- Clone GitHub Repository
```bash
git clone https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git
```

3- Node Js insallation packages and Nginx
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
sudo apt-get install nginx -y
```
4- Create RDS database
5- Create & Clone database
```bash
sudo apt install mysql-client-core-8.0
mysql -h database-1.cn44eiw2urw8.us-east-1.rds.amazonaws.com -u admin -p
create Database devops;
exit;
mysql -h database-1.cn44eiw2urw8.us-east-1.rds.amazonaws.com -u admin -p 12Qq1212 < users.sql
```
```bash
DB_HOST=database-1.cn44eiw2urw8.us-east-1.rds.amazonaws.com
DB_USER=admin
DB_PASSWORD=12Qq1212
DB_DATABASE=devops
```
6- Create and configure S3 bucket
```bash
{
  "Id": "Policy1732642401912",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1732642400094",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::blog-ammar/*",
      "Principal": "*"
    }
  ]
}
```
7- build and deploy to S3
```
npm run build
aws s3 sync ./dist s3://blog-ammar --region us-east-1
```
8- Enable Static Website Hosting
9- Start backend on EC2 

## Auto Scaling Launch template user data script
```
#!/bin/bash
# Update system packages
sudo apt-get update -y

# Navigate to the application directory
cd /home/ubuntu/React-NodeAPI-MySQL/backend

# Ensure PM2 is installed globally
sudo npm install -g pm2

# Restore PM2 processes if saved earlier
pm2 resurrect || pm2 start index.js --name "backend-app"

# Save the current PM2 process list (optional)
pm2 save

# Ensure PM2 starts on reboot
pm2 startup | sudo bash
```


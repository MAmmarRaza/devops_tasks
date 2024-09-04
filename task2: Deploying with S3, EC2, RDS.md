# Deploying frontend on S3, backend on EC2 and configuring database of RDS

## RDS
1- create RDS for mysql in free tier

2- sql configuration and creating database
```bash
mysql -h database-1.cl4suwy60mbo.us-east-1.rds.amazonaws.com -u admin -p
```
3- clone sql file in db-sql folder of backend
```bash
mysql -h database-1.cl4suwy60mbo.us-east-1.rds.amazonaws.com -u admin -p devops < users.sql
```

## EC2

1- Create Instance and connect with it, make sure security groups are configured correctly for 9001, 5713

2- clone repo
```bash
git clone https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git
```
3- Install NodeJs and npm as well as nginx
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
sudo apt-get install nginx
```
4- set env, Build and upload dist to s3
```bash
nano .env
http://server-ip:9001
npm run build
aws s3 sync ./dist s3://blog-ammar --region us-east-1
```

4- update sql connection in index.js file of backend

![image](https://github.com/user-attachments/assets/44516397-5f4a-4f37-a88a-939522aff44f)

## S3
1- Create S3 bucket and configure permission
```bash
{
  "Id": "Policy1725433843681",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1725433841167",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::blog-ammar/*",
      "Principal": "*"
    }
  ]
}
```
2- Open terminal and configure
```bash
sudo apt-get install awscli
aws configure
```
3- Build frontend and create dist folder

4- copy folder to s3 bucket
```bash
 aws s3 sync ./dist s3://blog-ammar --region us-east-1
```
5- Enable Static Website Hosting
- To serve your website from S3, you need to enable static website hosting:
  - Go to the Amazon S3 console.
  - Select the bucket where you uploaded your frontend build.
  - Go to Properties.
- Enable Static website hosting and set:
  - Index document: index.html
  - Error document (optional): index.html (for single-page apps)
    
 ![image](https://github.com/user-attachments/assets/51f91faa-b708-4b11-93cc-36124bc3da45)

6- Frontend Deployed on S3

![image](https://github.com/user-attachments/assets/0e645e40-89d6-4781-9922-3af5c244f114)

# Finally Running

![image](https://github.com/user-attachments/assets/d1378763-776b-41ea-8199-4cdfb35f52c4)


 

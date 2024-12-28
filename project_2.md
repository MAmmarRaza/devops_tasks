## Create Roles for services
1- EC2 Role
- AmazonEC2FullAccess
- AmazonEC2ContainerServiceforEC2Role
2- CodeDeploy Role
- AwsCodeDeployRole
- AmazonEC2RoleforAWSCodeDeploy

## EC2 Setup

1- NodeJs, NGinx setup
```
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

2- Code Deploy Agent 
```
sudo apt update
sudo apt install ruby-full
sudo apt install wget
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
systemctl status codedeploy-agent
```
3- Clone Repository and install packages
```
git clone https://github.com/MAmmarRaza/NodeJsProjectCMS.git
cd NodeJsProjectCMS
npm i
sudo npm i pm2 -g
pm2 start index.js
```

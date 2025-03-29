# CI CD of containerized Application on AWS using code deploy

## Create Launch Template
```
#!/bin/bash
sudo apt update -y
sudo apt install -y ruby-full
sudo apt install wget
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
systemctl status codedeploy-agent
#

sudo apt install -y docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker
echo "" | docker login -u "mammarraza" --password-stdin
sudo usermod -aG docker $USER
newgrp docker
docker run -d --name blog -p 80:5000 \
  -e MONGO_URL="mongodb+srv://20ntucs1120:WU6TAoQiyFljPJxO@blog-cluster.bvblv8k.mongodb.net/blog" \
  mammarraza/blog:latest
```

## Code Deploy work

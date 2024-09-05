# Creating CI / CD using Github  Actions

1- Installing Nodejs, npm packages
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
```

## configuring Github Actions

1- Go to setting -> actions -> runners -> new self-hosted-runner

![image](https://github.com/user-attachments/assets/668bd5b3-b792-4260-b2df-9ca6c1783146)

2- Run provided commands on server

![image](https://github.com/user-attachments/assets/f8e17423-74c4-4eaf-8f26-c530562fb58c)

```bash
sudo ./svc.sh install
sudo ./svc.sh start
```

3- GitHub Repository Settings:
- You define these secrets in your GitHub repository settings under Settings > Secrets and variables > Actions.
- In that section, you can add secrets by providing a name and value. For example:
- Secret Name: AWS_ACCESS_KEY_ID
- Secret Value: The actual AWS Access Key ID.

4- Create file and paste .github>workflows>main.yaml
```bash
name: s3-depl

on:
  push:
    branches: 
      - main

jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v3
      
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
          
      - name: Backend package install and restart
        run: |
          cd backend
          pwd
          npm install
          pm2 restart index
          cd ..
          
      - name: Build React App
        run: |
          cd frontend
          npm install
          npm run build
          ls
          pwd
        
      - name: Check Directory and Path
        run: |
          pwd
          ls
        
      - name: Navigate to Frontend
        run: |
          cd frontend
          ls
          pwd
        
      - name: Deploy app build to S3 bucket
        run: |
          aws s3 sync /home/ubuntu/actions-runner/_work/React-NodeAPI-MySQL/React-NodeAPI-MySQL/frontend/dist s3://myweb.com.cm --delete
```
4- Create bucket and make sure that you put the same name in last command of .yaml file

5- Commit in repo and push

6- you may face error for backend or pm2 error, its solution is you will see _work folder in actions-runner folder
```bash
cd _work/React-NodeAPI-MySQL/React-NodeAPI-MySQL/backend
npm i
pm2 start index.js
```

6- After connecting with database
![image](https://github.com/user-attachments/assets/e040dc9a-8869-4ead-b86b-9f81a4c1a816)

7- I did change in create button and when i commit it, it gets deployed too after running jobs

![image](https://github.com/user-attachments/assets/5d11385c-350c-4c8f-bc00-cc0f0e958d34)


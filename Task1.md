## Clone git repository

```bash
git clone https://github.com/HassanTariqAlvi/React-NodeAPI-MySQL
```

## Install and Configure SQL server
0- Installation
```bash
sudo apt-get update
sudo apt-get install mysql-server
sudo mysql_secure_installation
sudo systemctl status mysql
sudo systemctl start mysql
sudo systemctl enable mysql
```
1- Security
```bash
sudo mysql
USE mysql;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_new_password';
FLUSH PRIVILEGES;
exit
mysql -u root -p
```
2- Creating user and assigning previleges
```bash
CREATE USER 'devops-user'@'%' IDENTIFIED BY 'qwe123!@#QWE';
GRANT ALL PRIVILEGES ON `devops-db`.* TO 'devops-user'@'%';
```
3- Creating database and importing from host
```bash
create database `devops-db`;
sudo mysql -u root -p devops-db < users.sql
```

## Installing NodeJs, Npm package manager, Nginx
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
````
```bash
sudo apt-get update
sudo apt-get install nginx
```

## Dependencies and Running Backend and Frontend First

### backend
1- Install necessary dependencies
```bash
npm i pm2 -g
cd backend
npm install
nano index.js
```
2- Here update db connection configuartion like this:

![image](https://github.com/user-attachments/assets/a13babe7-d876-412e-870c-25a82cd7d910)

2- prun with pm2
```bash
pm2 start npm --name "my-backend" -- run start -- --port 9001
```
### frontend
Note: Make sure to set security groups inbound rules for 9001 and 5173 and 80 port
1- Install necessary dependencies and build
```bash
npm install
```
2- correct .env file
```
http://server-ip:9001
```

3- Test by dev running
```bash
pm2 start npm --name "my-frontend" -- run dev -- --port 5173
```
4- Now building frontend , it will give dist folder
```bash
npm run build
```

## Nginx Configuration
1- copy dist folder to /var/www/devops/frontend
```bash
cd /var/www
sudo mkdir devops
cd devops
sudo mkdir frontend
cd /homoe/ubuntu/React-NodeAPI-MySQL/frontend/
sudo cp -rf ./dist /var/www/devops/frontend
cd /var/www/devops/frontend
ls
```
2- configure nginx reverse proxy
```bash
sudo nano /etc/nginx/sites-available/devops
```
```bash
server {
    listen 80;
    server_name 54.82.14.15;  

    root /var/www/devops/frontend/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}
```
```bash
sudo ln -s /etc/nginx/sites-available/devops /etc/nginx/sites-enabled/devops
sudo systemctl restart nginx
```

# Finally
![image](https://github.com/user-attachments/assets/132b19ee-5bcd-4c29-9bc7-d67bcf1f28fd)








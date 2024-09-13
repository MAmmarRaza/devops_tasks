## Docker and Docker compose notes

sudo apt-get update
sudo apt install docker.io
sudo usermod -aG docker $USER
sudo systemctl restart docker

sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version

sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y

docker pull mysql:latest
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=12Qq1212 -p 3306:3306 -d mysql:latest

docker pull phpmyadmin
docker run --name phpmyadmin -d --link mysql-container:db -p 8081:80 phpmyadmin/phpmyadmin


CREATE TABLE `users` (
  `id` int(255) NOT NULL,
  `name` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `email` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `work` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

ALTER TABLE `users`
  ADD PRIMARY KEY (`id`);
 
ALTER TABLE `users`
  MODIFY `id` int(255) NOT NULL AUTO_INCREMENT;

git clone https://github.com/MAmmarRaza/React-NodeAPI-MySQL.git

docker build -t mammarraza/back:1.0 .
docker run -p 9001:9001 -e DB_HOST=139.59.44.16 -e DB_USER=root -e DB_PASSWORD=12Qq1212 -e DB_DATABASE=devops-db mammarraza/back:1.0


docker build -t mammarraza/front:1.0 .
docker run -d -p 4173:4173 -e VITE_BASE_URL="http://139.59.44.16:9001" -e VITE_HOST="0.0.0.0" mammarraza/front:3.0

sudo nano /etc/nginx/sites-available/exportthreads

server {
    listen 80;
    server_name backend.exportthreads.live;

    location / {
        proxy_pass http://139.59.44.16:9001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

sudo ln -s /etc/nginx/sites-available/exportthreads /etc/nginx/sites-enabled/

sudo nano /etc/nginx/sites-available/backend
sudo ln -s /etc/nginx/sites-available/backend /etc/nginx/sites-enabled/





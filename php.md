## Mysql Installation
```
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql
```


## php composer installation
```
sudo apt update
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y

sudo add-apt-repository ppa:ondrej/php

sudo apt update
sudo apt install php8.2 php8.2-cli -y
sudo apt install php8.2-fpm -y
sudo apt install php8.2-{mysql,curl,gd,mbstring,xml,zip,opcache,intl,bcmath} -y

sudo systemctl start php8.2-fpm
sudo systemctl enable php8.2-fpm
sudo systemctl status php8.2-fpm

```

## Insatalling composer
```
# 1. Download the installer script
curl -sS https://getcomposer.org/installer -o composer-setup.php

# 2. Run the installer script using PHP
php composer-setup.php

sudo mv composer.phar /usr/local/bin/composer
```

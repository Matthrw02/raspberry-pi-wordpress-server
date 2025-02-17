# Raspberry Pi WordPress Server  
Self-hosted WordPress server using Raspberry Pi 4B, Nginx, MariaDB, and PHP.  

## Overview  
This project demonstrates how to set up a web server on a Raspberry Pi 4B for hosting WordPress. It uses Nginx as the web server, MariaDB as the database server, and PHP for backend processing.  

## Hardware  
- Raspberry Pi 4B  
- Sandisk 128GB microSD card  

## Software Stack  
- Raspberry Pi OS Lite 64-bit  
- Nginx (Web Server)  
- MariaDB (Database Server)  
- PHP (Backend)  
- WordPress (CMS)  

## Setup Process  
### 1. Prepare Raspberry Pi  
- Flash Raspberry Pi OS Lite 64-bit onto the microSD card.  
- Connect via Ethernet and enable SSH for headless operation.  
- Update the system:  
  ```bash
  sudo apt update && sudo apt upgrade -y

### 2. Install Web Server and Database
```
sudo apt install nginx -y
sudo apt install mariadb-server -y
sudo apt install php-fpm php-mysql -y
```
 
### 3. Download and Set Up WordPress
```
cd /var/www/
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo rm latest.tar.gz
sudo chown -R root:root wordpress/
```
### 4. Configure Nginx
Edit the Nginx configuration:
```
upstream wp-php-handler {
    server unix:/var/run/php/php8.2-fpm.sock;
}
server {
    listen 80;
    server_name _;
    root /var/www/wordpress/;
    index index.php;
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass wp-php-handler;
    }
}
```
### 5. Create a Database for WordPress
```   
sudo mysql
create database wordpress default character set utf8 collate utf8_unicode_ci;
create user 'username'@'localhost' identified by 'password';
grant all privileges on wordpress.* TO 'username'@'localhost';
flush privileges;
exit
```
### 6. Finalize WordPress Installation
- Access the Raspberry Pi’s IP address in a web browser.
- Enter database credentials and complete the setup.

### Future Plans
This setup was done for testing purposes. The web server will be moved to a different machine, and the Raspberry Pi will be repurposed for other projects.

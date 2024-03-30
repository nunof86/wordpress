# Documentation

## See the full documentation on https://documentation-com.gitbook.io/wordpress-installation/

# Prerequisites

## System Update

```bash
sudo apt update && sudo apt full-upgrade -y
```

## Install Required Packages

```bash
sudo apt install apt-transport-https ca-certificates software-properties-common -y
```

## Docker Installation - Ubuntu

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## Docker Installation - Debian

### **Add Docker APT Key**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
```

### **Add Docker APT Repository**

```bash
echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```

## **Install Docker**

```bash
sudo apt-get update
sudo apt-get install docker-ce -y
```

## **Download and Install Docker Compose**

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64" -o /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
```

## Ansible Installation

```bash
sudo apt install ansible -y
```

# WordPress Docker Deployment

## Image Pull

1. First pull the latest WordPress image.

```bash
docker pull wordpress:latest
```

## Configuration of the docker-compose file

1. First create a new directory named <mark style="color:red;">`wordpress`</mark> and inside that directory create a <mark style="color:red;">`docker-compose.yml`</mark> file.

```bash
mkdir wordpress
cd wordpress
nano docker-compose.yml
```

2. After that populate the file with the information bellow, you can change the <mark style="color:red;">`ports:`</mark> and some <mark style="color:red;">**credentials**</mark>.

```yaml
version: '3.9'
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: your_wordpress_db_name
      WORDPRESS_DB_USER: your_wordpress_db_user
      WORDPRESS_DB_PASSWORD: your_wordpress_db_password

  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: your_mysql_root_password
      MYSQL_DATABASE: your_wordpress_db_name
      MYSQL_USER: your_wordpress_db_user
      MYSQL_PASSWORD: your_wordpress_db_password

volumes:
  db_data:
```

## Docker Compose

1. Start the WordPress deployment using docker-compose:

```bash
docker-compose up -d
```

2. After that navigate to the dashboard with <mark style="color:red;">`http://your_ip_address:8080`</mark>.

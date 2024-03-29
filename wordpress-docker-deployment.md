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

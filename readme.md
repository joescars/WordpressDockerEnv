# Wordpress Local Development using Docker

This docker-compose file sets up a local WordPress instance loading the website files from your local drive and data a docker volume. 

I've found this setup works best for me when I need to work on my custom WordPress site locally. 

## Instructions

1. Clone the repo
2. Run the following command
```
docker-compose up -d
```

## Additional Info

```yml
  mysql:
    image: mysql:5.7
    restart: always
    volumes:
      - db-data:/var/lib/mysql    
    ports:
      - 8086:3306
    environment:
      MYSQL_ROOT_PASSWORD: some_password
```

Creates mysql container with a default root password you define

```yml
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - 8081:80
    environment:
      MYSQL_ROOT_PASSWORD: password123
    links:
      - mysql:db
```

Creates a container running phpMyAdmin pointing to your mysql container so you can administer it. 

```yml
  web:
    image: wordpress
    volumes:
      - ./wordpress:/var/www/html
    restart: always
    links:
      - mysql
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: your_user
      WORDPRESS_DB_NAME: your_db_name
      WORDPRESS_DB_PASSWORD: your_pass     
```

Creates a wordpress container that links to your mysql insance along with the credentials you supply. This pulls your wordpress installation files from a local folder (optional) so you can develop locally. 

```yml
volumes:
 db-data:
```

Stores mysql data in docker volume. 
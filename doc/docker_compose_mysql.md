# Docker Compose MySQL

[Back](../README.md)

- [Docker Compose MySQL](#docker-compose-mysql)
  - [Define Compose File](#define-compose-file)
  - [Create Image and Container](#create-image-and-container)
  - [ECR](#ecr)

---

## Define Compose File

```yaml
version: "3"

services:
  # service of mysql
  mysql:
    # specify env var
    environment:
      # specify root pwd
      MYSQL_ROOT_PASSWORD: welcome
      # specify a user
      MYSQL_USER: mysql_dba
      # specify pwd
      MYSQL_PASSWORD: welcome
      # specify db
      MYSQL_DATABASE: my_db
    # Specify container name
    container_name: mysql_con
    # specify base image
    image: mysql:8.1
    # Specify volume for consistency
    volumes:
      # map volume to a path
      - mysql_volume:/var/lib/mysql
    # Specify port to expose to external
    ports:
      # external port: internal
      - 3307:3306
    # Specify network
    networks:
      - mysql_network

volumes:
  mysql_volume:

networks:
  mysql_network:
```

---

## Create Image and Container

```sh
# create image and container
docker compose up -d

# list all containers
docker compose ps

# all container down and remove volume
docker compose down -v 
```

- Test:
  - connect to mysql 

```sh
mysql -u mysql_dba -P 3307 -h 127.0.0.1 -p
```

---

## ECR

```sh
## Push to AWS ECR

```sh
aws ecr get-login-password --region us-east-1 |  docker login --username AWS --password-stdin 099139718958.dkr.ecr.us-east-1.amazonaws.com

# # create repo
# aws ecr create-repository --repository-name docker/mysql
# aws ecr describe-repositories

# Build your Docker image
docker compose up -d

docker images ls

# tag
docker tag ae2502152260 099139718958.dkr.ecr.us-east-1.amazonaws.com/docker/mysql:latest

# push this image to newly created AWS repository
docker push 099139718958.dkr.ecr.us-east-1.amazonaws.com/docker/mysql:latest
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'welcome';

CREATE USER 'mysqldba'@'%' IDENTIFIED BY 'welcome';
GRANT ALL PRIVILEGES ON *.* TO 'mysqldba'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
# blogs-post-flask-mysql


## Docker

First create a docker image from Dockerfile

```bash
docker build -t flaskapp .
```

Now, make sure that you have created a network using following command

```bash
docker network create twotier
```

Attach both the containers in the same network, so that they can communicate with each other

i) MySQL container

```bash
docker run -d \
    --name mysql \
    -v mysql-data:/var/lib/mysql \
    --network=twotier \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql:5.7
```

ii) Backend container

```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    flaskapp:latest
```

### Access MySQL database

Now Let’s get in to database container and see whether the data get loaded correctly

```bash
$docker exec -it mysql bash
bash-5.1# mysql -u root -p
Enter password:

mysql> SHOW databases;
```

Now we can access the application in browser <http://localhost:5000>

## DOCKER-COMPOSE

In order to run the our dockerized app, we will execute the following command from the terminal docker:

```bash
compose up -d
```

Check the status of container

```bash
docker ps
```
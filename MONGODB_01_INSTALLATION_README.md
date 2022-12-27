# Setup environment

**Pre-requisites**: 
- Docker installed (https://hub.docker.com/_/mongo)


```shell
# Let's pull the MongoDB image from the docker repository
docker pull mongo

# Creating and running MongoDB image named mongodb-shell
docker run -it --name mongodb-shell mongo

# Stopping the MongoDB image
docker stop mongodb-shell
```

```shell
# Starting the previously created container
docker start mongodb-shell

# Accessing the MongoDB shell in the image
docker exec -it mongodb-shell mongosh
```

```shell
/* From within the shell, list the databases in MongoDB */
show dbs
```

```shell
/* Exiting the MongoDB shell */
exit
```

```shell
# Stopping the MongoDB image
docker stop mongodb-shell
```

```shell
# Accessing MongoDB log from Docker
docker logs mongodb-shell
```
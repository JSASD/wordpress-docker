# docker-wordpress
 WordPress server running in docker with Swarm Secrets


# Prerequisites

 - [Install Docker](https://docs.docker.com/engine/install/)
 - [Install Docker Compose](https://docs.docker.com/compose/install/)

## Optional: Add user to Docker group

If you want to interact with Docker without running `sudo`, add your current user to the Docker group with `sudo usermod -aG docker YOURUSER`

***NOTE:*** *The rest of this document will assume this is done. If you didn't add your user to the group, just preface all `docker` commands with `sudo`*


# Create Swarm

To securely manage passwords (using [docker secrets](#create-docker-secrets)), you'll need to put your machine into Swarm Mode. Don't worry though, you can run a swarm with a single node, but this is required to use `docker secrets`.

To do this, just run `docker swarm init`



# Prepare container

## Create Docker Compose File

From the contents of the compose file from this repository, add and modify as needed.

## Create docker secrets

Docker secrets are a secure way to store sensitive data. Create them with the following commands, filling in your desired passwords:

```sh
echo P@55w0rd | docker secret create db_root_password -
# db_root_password
echo P@55w0rd | docker secret create db_password -
# db_password
```

## Create docker volumes

You may have noticed the volumes used in the `compose` file are external. We will need to create these:

```sh
docker volume create db_data
# db_data
docker volume create wp_data
# wp_data
```

## Deploy stack

If you are familiar with Kubernetes (`kubectl`), this is a very similar process as a deployment.

To deploy the stack defined in your `docker-compose.yml` file:

```sh
docker stack deploy --compose-file=docker-compose.yml wordpress
# Creating network wordpress_mysql_private
# Creating service wordpress_wordpress
# Creating service wordpress_db
```


# Useful Commands

## List running containers
```sh
docker container ls
```

## List deployments
```sh
docker stack ls
# Removing service wordpress_db
# Removing service wordpress_wordpress
# Removing network wordpress_mysql_private
```

## Remove deployment
```sh
docker stack rm wordpress
# wordpress
```


# Attributions

Allan MacGregor at [earthly.dev](https://earthly.dev/blog/docker-secrets/) for the epic help with docker secrets with WordPress
[James Walker](https://www.cloudsavvyit.com/author/jameswalker/) at [cloudsavvyit.com](https://www.cloudsavvyit.com/) for more help with docker secrets
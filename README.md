# docker-wordpress
 WordPress server running in docker with Swarm Secrets


# Prerequisites

 - [Install Docker](https://docs.docker.com/engine/install/)
 - [Install Docker Compose](https://docs.docker.com/compose/install/)

## Optional: Add user to Docker group

If you want to interact with Docker without running `sudo`, add your current user to the Docker group with `sudo usermod -aG docker YOURUSER`

***NOTE:*** *The rest of this document will assume this is done. If you didn't add your user to the group, just preface all `docker` commands with `sudo`*


# Create Swarm

To securely manage passwords, you'll need to put your machine into Swarm Mode. Don't worry though, you can run a swarm with a single node, but this is required to use `docker secrets`.

To do this, just run `docker swarm init`


# Create necessary secrets

 - MYSQL_ROOT_PASSWORD
   - `echo P@55w0rd | docker secret create db_root_password -`
 - MYSQL_PASSWORD
   - `echo P@55w0rd | docker secret creat db_password -`


# Prepare container

## Create Docker Compose File

From the contents of the compose file from this repository, add and modify as you need

## Create the necessary directories

In the same directory as you compose file, you'll want to create a `data` directory, inside of there you'll want to create two more directories: `wordpress_data` and `db_data`.

So, the directory structure in my case looks as such:

```
📦homeDir/
├─ 📂wordpress/
│  ├─ 📜docker-compose.yml
│  ├─ 📂data/
│  │  ├─ 📂wordpress_data/
│  │  ├─ 📂db_data/
```

# Attributions

[earthly.dev](https://earthly.dev/blog/docker-secrets/) for the epic help with docker secrets with WordPress
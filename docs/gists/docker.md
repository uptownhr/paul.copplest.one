---
description: Some useful docker commands
---
# Docker

## Docker
```bash
# Create
docker build -t appname .

# Start
docker run appname

# Stop all containers
docker stop $(docker ps -a -q)

# Kill all containers
docker kill $(docker ps -q)

### Remove all containers
docker rm $(docker ps -a -q)

# Remove all docker images
docker rmi $(docker images -q)
```

## Docker Compose
```bash
## Start
docker-compose up

## Start detached
docker-compose up -d

## List
docker-compose ps

## Rebuild
docker-compose up --force-recreate --build
```

## Docker Machine


#### Create new server

```bash


# Override default profile
AWS_ACCESS_KEY_ID=$(aws --profile PROFILE_NAME configure get aws_access_key_id)
AWS_SECRET_ACCESS_KEY=$(aws --profile PROFILE_NAME configure get aws_secret_access_key)
docker-machine create \
  --driver amazonec2 \
  --amazonec2-region ap-southeast-1 \
  --amazonec2-open-port 3001 \
  --amazonec2-access-key $AWS_ACCESS_KEY_ID \
  --amazonec2-secret-key $AWS_SECRET_ACCESS_KEY \
  --amazonec2-instance-type t2.nano \
  machine-name

docker-machine ls # check that it exists

eval $(docker-machine env machine-name) # attach to machine
docker-compose up -d # deploy on machine
eval "$(docker-machine env -u)" # attach to local again
```

#### Update

```bash
eval $(docker-machine env machine-name)   # attach to machine
docker-compose build --no-cache           # deploy on machine
docker-compose up -d                      # make sure it's up and reloads config
eval "$(docker-machine env -u)"           # attach to local again
```

This will destroy the redis instance: `docker-compose up --force-recreate`

#### See Logs

```bash
eval $(docker-machine env machine-name) # attach to machine
docker ps # see list of machines
docker logs __ID__ # see logs
eval "$(docker-machine env -u)" # attach to local again
```

#### Remove server

```bash
docker-machine rm machine-name
```

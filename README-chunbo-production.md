
# README for Chunbo redash production service

I need modify the redash source and compose in our redash production system, so I adjust the docker setup files, eg: uncommenti `ldap3` and comment out `ibm-db` python dependency libraries.
Notice: 
* `docker-compose-chunbo-production.yml` file modified from the production redash setup [docker-compose.yml](https://github.com/getredash/setup/blob/master/data/docker-compose.yml).
* `SimbaSparkODBC-2.6.10.1010-2-Debian-64bit.zip` file need download manually because everybody know the reason, AWS S3 url is forbidden to download.


## Method One
### build the docker image
```
 nohup docker build --build-arg skip_dev_deps=true -f ./Dockerfile-chunbo-production -t redash_source_server:latest .
```

### rename the docker tag if build have no image tag
```
 docker tage <image_id> redash_source_server:latest
```


### restart the docker production server 
```
# when use the docker-compose-chunbo-production.yml directly
 docker-compose -f docker-compose-chunbo-production.yml up -d --force-recreate --no-deps server
 docker-compose -f docker-compose-chunbo-production.yml up -d --force-recreate --no-deps adhoc_worker
 docker-compose -f docker-compose-chunbo-production.yml up -d --force-recreate --no-deps scheduled_worker
 docker-compose -f docker-compose-chunbo-production.yml up -d --force-recreate --no-deps scheduler

# when rename the docker-compose-chunbo-production.yml to docker-compose.yml  
 docker-compose -f docker-compose-chunbo-production.yml up -d --force-recreate --no-deps server
 docker-compose up -d --force-recreate --no-deps adhoc_worker
 docker-compose up -d --force-recreate --no-deps scheduled_worker
 docker-compose up -d --force-recreate --no-deps scheduler
```
## Method Two
### docker compose build 
```
docker-compose -f docker-compose-chunbo-production.yml build
```
### start all the docker-compose services 
```
docker up -d
```

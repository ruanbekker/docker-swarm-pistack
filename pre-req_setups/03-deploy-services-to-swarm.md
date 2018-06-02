
## Deploy some services to your Swarm:

Get the compose files:

```
git clone https://github.com/ruanbekker/docker-swarm-pistack.git
cd docker-swarm-pistack
```

Create the Docker Overlay Network:

```
$ docker network create --driver overlay docknet
```

Deploy the Traefik Service:

```
$ docker stack deploy -c docker-compose-traefik.yml proxy
```

Deploy the Database Services:

```
$ docker stack deploy -c docker-compose-dbs.yml dbs
```

Deploy the Web Services:

```
$ docker stack deploy -c docker-compose-web.yml apps
```

## Verify

List your stacks:

```
$ docker stack ls
NAME                SERVICES
proxy               1
dbs                 2
apps                3
```

And check the services running in the stacks:

```
$ docker stack services dbs
ID                  NAME                MODE                REPLICAS            IMAGE                          PORTS
ew3vcjeo7ind        dbs_redis           replicated          1/1                 rbekker87/redis:alpine-armhf
pmafyvyrgopc        dbs_minio           replicated          1/1                 pixelchrome/minio-arm:latest
```

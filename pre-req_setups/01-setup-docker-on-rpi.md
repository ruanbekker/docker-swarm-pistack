## Flashing the RaspberryPi:

Lately I've been using Hypriot, so I currently its between these 2:

1. [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/)
2. [Hypriot Docker Image](https://blog.hypriot.com/downloads/)

## Setup Docker:

Hypriot comes with Docker preloaded, if you use Raspbian Lite:

```
$ sudo apt update && sudo apt upgrade -y
$ curl https://get.docker.com | sudo bash
$ sudo usermod -aG docker pi
```

## Setup Docker Swarm:

On my setup I have a 1 manager / 3 workers setup, so on the manager node, initialize the swarm, 
then copy the join token to join the workers to the swarm.

```
$ docker swarm init --advertise-addr eth0
docker swarm join \    
   --token SWMTKN-1-<long-token-string> \
   <managers-eth0-ip>:2377
```

With the returned output, copy that and execute it on your worker nodes, to join your workers to the swarm:

```
$ docker swarm join \    
   --token SWMTKN-1-<long-token-string> \
   <managers-eth0-ip>:2377
This node joined a swarm as a worker.
```

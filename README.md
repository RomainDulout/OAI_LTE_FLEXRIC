# OAI_LTE_FLEXRIC
OpenAirInterface simulator LTE-RAN + EPC + FlexRAN for multilayer approach in cellular networks


# TUTO DOCKER / DOCKER-COMPOSE

# --> Docker Network 
docker network prune 

--> Commande pour créer un bridge avec docker

docker network create -d bridge --subnet 192.168.100.0/24 simu-network 

--> Commande pour ajouter un HostName

docker run -tid --name app2 --network simu-network --ip 192.168.100.200 --hostname ROMY alpine

# --> Docker Environnment variables
docker run -tid --name test --env MYVARIABLE="Bonjour" ubuntu:latest

# --> Docker Volume
flag -v host-volume:container-volume

docker volume create

docker run -tid --name test -p 8080:80 --mount source=volume-test,target=/usr ubuntu:latest

# --> Dockerfile

FROM ubuntu:latest
RUN apt-get update \
    && apt-get install -y nano git \
    && apt-get clean \
    && rm -fr /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD [ "google.fr" ]
ENTRYPOINT [ "ping" ]

# --> Docker Compose 

--> Fichier à écrire:
docker-compose.yml

--> Commandes utiles: 
docker-compose build (construction des images)
docker-compose up -d (build + run + detach)
docker-compose ps (état des services)
docker-compose start/stop/rm

--> Fichier de base example: 
version: '3.9'

services: 
    myfirstservice: 
        image: alpine
        restart: always
        container_name: MyAlpine
        entrypoint: ps aux



# STEPS TO DO 

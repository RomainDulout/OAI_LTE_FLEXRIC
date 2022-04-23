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

0. Mandatory Tasks 
    
> upgrade docker-compose >1.27

sudo apt update
sudo apt upgrade
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

> forwarding

sudo sysctl net.ipv4.conf.all.forwarding=1
sudo iptables -P FORWARD ACCEPT

> pull some images

docker pull ubuntu:bionic
docker pull cassandra:2.1
docker pull redis:6.0.5

> create special network

sudo nano /etc/docker/daemon.json
{
    "bip":192.168.17.1/24
}

> restart

sudo service docker restart
docker info

2. Download/Pull/Tag docker images

docker pull rdefosseoai/oai-hss:latest
docker pull rdefosseoai/oai-spgwc:latest
docker pull rdefosseoai/oai-spgwu-tiny:latest
docker pull rdefosseoai/magma-mme:latest

docker image tag rdefosseoai/oai-hss:latest oai-hss:production
docker image tag rdefosseoai/oai-spgwc:latest oai-spgwc:production
docker image tag rdefosseoai/oai-spgwu-tiny:latest oai-spgwu-tiny:production
docker image tag rdefosseoai/magma-mme:latest magma-mme:master

3. Running 

docker-compose up -f db_init
(pour debug: OK: docker logs demo-db-init --follow)
docker rm -f demo-db-init
docker logs demo-cassandra
(....
INFO  20:19:40 Initializing vhss.extid_imsi_xref)

4. DEPLOY

docker-compose up -d oai_spgwu

5. Undeploy EPC
docker-compose down







cd /tmp/

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.7/linux-headers-4.7.0-040700_4.7.0-040700.201607241632_all.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.7/linux-headers-4.7.0-040700-generic_4.7.0-040700.201607241632_amd64.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.7/linux-image-4.7.0-040700-generic_4.7.0-040700.201607241632_amd64.deb

sudo dpkg -i *.deb

sudo add-apt-repository ppa:cappelikan/ppa
sudo apt update
sudo apt install mainline

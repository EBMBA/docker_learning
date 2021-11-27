# Beginning 

## 1. Installation 

```bash
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world
```

## 2. Upgrade
```bash 
sudo apt-get update

sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world

Add current user to Docker group :
sudo usermod -aG docker $USER

```

## 3. Editions & Versions

Editions :
-   Docker Community Edition (CE) &rarr;  For **single dev** or **small teams**. **Free edition**.
-   Docker Enterprise Edition (EE) &rarr; Big teamps in corp and for **large scale prod**. 

Versions :
-   Stable  &rarr; Latest releases
-   Test    &rarr; Pre-releases redy to be test 
-   Nightly &rarr; Latest build version to next release 


## 4. Commands

Search official image : `docker search --filter "is-official=true" ubuntu`

Download image (latest) : `docker pull ubuntu` or specific version `docker pull ubuntu:16.04`

Display all image installed : `docker images`

Remove image (with):
-   ID : `docker rmi ID`
-   name : `docker rmi name`
-   force : `docker rmi -f name`
-   all images : `docker rmi -f $(docker images -q)`

Display :
-   Help : `docker help` or `docker <command> --help`
-   Version / info on docker: `docker --version` or `docker info`

Run image : `docker run <options> name`
-   `-t` : Allocate a TTY nickname
-   `-i` : Keep a STDIN opened
-   `-d` : Execute container in the background and display container ID
-   `--name` : Give a name to the container
-   `--expose` : Open specific port or range of port
-   `-p` or `--publish` : To do port forwarding (port have to be exposed)

List images : `docker images ls` or `docker images`

Create new image with a container : `docker commit <CONTAINER NAME or ID> <NEW IMAGENAME>`

Build a new image from Dockerfile : `docker build -t <IMAGE NAME> .` 


Container :
-   List container : `docker container ls` or `docker ps`
-   Remove container : `docker rm <CONTAINER NAME or ID>`
-   Execute command in container : `docker exec -ti <CONTAINER NAME> /bin/bash`
-   To quit container without remove it : `Ctrl + P +Q`
-   Display logs : `docker logs -ft <CONTAINER NAME>`  with `-f` to follow in live logs and `-t` to display hours and minutes 



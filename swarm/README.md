# SWARM 

## Configuration 

1. Create VM 
```Docker
docker-machine create --driver vmware mymanager 
docker-machine create --driver vmware worker
docker-machine create --driver vmware worker2
docker-machine create --driver vmware worker3
```

2. IP Configuration
```Docker
docker-machine ls

> 
    NAME        ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER      ERRORS
    mymanager   -        vmware   Running   tcp://172.16.67.131:2376           v19.03.12   
    worker      -        vmware   Running   tcp://172.16.67.132:2376           v19.03.12   
    worker2     -        vmware   Running   tcp://172.16.67.133:2376           v19.03.12   
    worker3     -        vmware   Running   tcp://172.16.67.134:2376           v19.03.12   
```

3. Activate Docker Swarm 
```Docker
docker-machine ssh mymanager "docker swarm init --advertise-addr 172.16.67.131"

>
    Swarm initialized: current node (i7yw9nc3lcszcm2wh5a7hu3zc) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-4qvl8fa3chy5qx9sxfg8x0vlijuy4tm2yjn79kuxsehkltdjlp-0jm4fv81cz6849bhbyhwd7l7o 172.16.67.131:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

4. Add workers in Swarm cluster
```Docker 
docker-machine ssh worker "docker swarm join --token SWMTKN-1-4qvl8fa3chy5qx9sxfg8x0vlijuy4tm2yjn79kuxsehkltdjlp-0jm4fv81cz6849bhbyhwd7l7o 172.16.67.131:2377"

docker-machine ssh worker2 "docker swarm join --token SWMTKN-1-4qvl8fa3chy5qx9sxfg8x0vlijuy4tm2yjn79kuxsehkltdjlp-0jm4fv81cz6849bhbyhwd7l7o 172.16.67.131:2377"

docker-machine ssh worker3 "docker swarm join --token SWMTKN-1-4qvl8fa3chy5qx9sxfg8x0vlijuy4tm2yjn79kuxsehkltdjlp-0jm4fv81cz6849bhbyhwd7l7o 172.16.67.131:2377"

docker-machine ssh mymanager "docker node ls"

>
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
    i7yw9nc3lcszcm2wh5a7hu3zc *   mymanager           Ready               Active              Leader              19.03.12
    vz2bdqd82avknddlivx1vjlz1     worker              Ready               Active                                  19.03.12
    ktrby2jefmw0lr40v24rbey6y     worker2             Ready               Active                                  19.03.12
    mlf8wati44bvp2frh1zkxar6p     worker3             Ready               Active                                  19.03.12
```

5. Activate Docker Machine interface from master
```
eval $(docker-machine env mymanager)

docker-machine active

> 
    mymanager
```

## Deploy an apps

1. Create a service

A service can define (for a container) :
-   The image name and tag that the node containers must execute;
-   How many containers are involved in the service;
-   The ports to be exhibited outside the Swarm cluster;
-   How the container should act following an error;
-   The characteristics of the nodes on which the service can run.

```Docker
docker service create --name flaskc \
--replicas 3 \
--publish published=5001,target=5000 \
--restart-condition=on-failure \
--limit-memory	100M \
hajdaini/flask:first

docker service ls
>
    ID             NAME      MODE         REPLICAS   IMAGE                  PORTS
    m8q5r92gdbo7   flaskc    replicated   3/3        hajdaini/flask:first   *:5001->5000/tcp

docker service ps flaskc
>
    ID             NAME       IMAGE                  NODE        DESIRED STATE   CURRENT STATE                ERROR     PORTS
    xx417rgy3f1w   flaskc.1   hajdaini/flask:first   worker      Running         Running about a minute ago             
    v1sahnhcn745   flaskc.2   hajdaini/flask:first   worker2     Running         Running about a minute ago             
    ltnw5dxty08v   flaskc.3   hajdaini/flask:first   mymanager   Running         Running about a minute ago     
```

2. Scale-up
```Docker 
docker service scale flaskc=5

docker service ps flaskc

> 
    ID             NAME       IMAGE                  NODE        DESIRED STATE   CURRENT STATE            ERROR     PORTS
    xx417rgy3f1w   flaskc.1   hajdaini/flask:first   worker      Running         Running 9 minutes ago              
    v1sahnhcn745   flaskc.2   hajdaini/flask:first   worker2     Running         Running 9 minutes ago              
    ltnw5dxty08v   flaskc.3   hajdaini/flask:first   mymanager   Running         Running 9 minutes ago              
    wefjh01wpxnu   flaskc.4   hajdaini/flask:first   worker3     Running         Running 27 seconds ago             
    8c6z7lpa96zp   flaskc.5   hajdaini/flask:first   worker3     Running         Running 27 seconds ago
```

3. Update without service interruption

```Docker
docker service update --image hajdaini/flask:second flaskc
```

4. Delete service 
```Docker 
docker service rm flaskc
```

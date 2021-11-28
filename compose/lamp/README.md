```docker
docker build -t myapp .

docker-compose up -d

docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                                   NAMES
805f5657a96f   myapp       "docker-php-entrypoi…"   13 seconds ago   Up 12 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   myapp_c
cf6003051268   mysql:5.7   "docker-entrypoint.s…"   14 seconds ago   Up 13 seconds   3306/tcp, 33060/tcp                     mysql_c



docker-compose ps
 Name                Command               State                  Ports                
---------------------------------------------------------------------------------------
myapp_c   docker-php-entrypoint apac ...   Up      0.0.0.0:8080->80/tcp,:::8080->80/tcp
mysql_c   docker-entrypoint.sh mysqld      Up      3306/tcp, 33060/tcp  

docker-compose logs

```
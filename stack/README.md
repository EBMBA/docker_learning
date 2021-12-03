# DOKCER STACK

1. Deploy app multi-services
```Docker
docker stack deploy -c docker-compose.yml api-app

docker stack ps api-app
>
    ID             NAME             IMAGE                 NODE        DESIRED STATE   CURRENT STATE            ERROR     PORTS
    nv58o2chm0dd   api-app_api1.1   hajdaini/flask:api1   worker3     Running         Running 22 seconds ago             
    odfvuawbhni2   api-app_api1.2   hajdaini/flask:api1   worker      Running         Running 22 seconds ago             
    2iw6hycmoed0   api-app_api1.3   hajdaini/flask:api1   mymanager   Running         Running 22 seconds ago             
    skovkawvl2ru   api-app_api2.1   hajdaini/flask:api2   worker      Running         Running 24 seconds ago             
    n7aeqo38k6mv   api-app_api2.2   hajdaini/flask:api2   worker2     Running         Running 24 seconds ago             
    ssdlbaofttxj   api-app_api2.3   hajdaini/flask:api2   mymanager   Running         Running 24 seconds ago   

docker stack rm api-app
```
```docker
sudo docker build -t my_lamp_v2 .

docker volume create --name mysqldata 

docker run -d --name my_lamp_c -v $PWD/app:/var/www/html -v mysqldata:/var/lib/mysql -p 8080:80 my_lamp_v2
```
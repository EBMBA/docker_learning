```docker
docker pull httpd:latest

docker run --name webServer -p 8081:80 httpd:latest
"or to run it in the background" 
docker run --name webServer -d -p 8081:80 httpd:latest

docker exec -ti webServer /bin/bash 

    echo "<h1> Docker it's really cool</h1>" > /usr/local/apache2/htdocs/index.html


```
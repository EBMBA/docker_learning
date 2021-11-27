```docker
docker pull httpd:latest

docker run -ti --name webServer -p 8080:80 httpd:latest
"or to run it in the background" 
docker run -ti --name webServer -d -p 8080:80 httpd:latest

docker exec -ti webServer /bin/bash 

    echo "<h1> Docker it's really cool</h1>" > /usr/local/apache2/htdocs/index.html


```
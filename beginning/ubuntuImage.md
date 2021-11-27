Create a new image with container 
```docker 
docker run -ti --name myUbuntu ubuntu:latest

    apt-get update -y && apt-get install -y git
    echo "test file" > test.txt && cat test.txt

    (CRTL+P+Q)

docker commit myUbuntu ubuntugit

docker images

docker run -ti --name ubuntugit_container ubuntugit

    cat test.txt
    >   test file

```
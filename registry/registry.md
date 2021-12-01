docker run -d -p 5000:5000 --restart=always --name my_registry registry:2
docker build -t alpinegit .
docker tag alpinegit localhost:5000/alpinegit
docker push localhost:5000/alpinegit

----------------
New secure registry

Create certificat : 

openssl req \
    -newkey rsa:4096 -nodes -sha256 -keyout certs/localhost.key \
    -x509 -days 365 -out certs/localhost.crt

Generate htpasswd file :

docker run --rm \
    --entrypoint htpasswd \
    registry:2 -Bbn testuser testpassword > auth/htpasswd

docker run -d \           
    --restart=always \   
    --name mon-registry \
    -v "$(pwd)"/data:/var/lib/registry \
    -v "$(pwd)"/certs:/certs \         
    -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
    -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/localhost.crt \
    -e REGISTRY_HTTP_TLS_KEY=/certs/localhost.key \
    -p 443:443 \
    registry:2

docker run -d \
    --restart=always \
    --name registry \
    -v "$(pwd)"/data:/var/lib/registry \
    -v "$(pwd)"/auth:/auth \
    -v "$(pwd)"/data:/var/lib/registry \
    -v "$(pwd)"/certs:/certs \         
    -e REGISTRY_HTTP_ADDR=0.0.0.0:443" \
    -e "REGISTRY_AUTH=htpasswd" \
    -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
    -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
    -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/localhost.crt \
    -e REGISTRY_HTTP_TLS_KEY=/certs/localhost.key \
    registry:2

docker tag alpinegit localhost:443/alpinegit
docker push localhost:443/alpinegit
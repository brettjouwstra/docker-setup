# Local Docker Registry

You will need to have [mkcert](https://github.com/FiloSottile/mkcert) installed or your own means of providing certificates. 

### Configure Host Directory

```
mkdir /mnt/user/docker_registry && cd /mnt/user/docker_registry

mkdir auth certs storage
```

### Generate Certificates 

```
cd certs

mkcert reg.server.local 192.168.1.4

>>> The certificate is at "./reg.server.local+1.pem" and the key at "./reg.server.local+1-key.pem" 

cd ..
```

### Create an htpasswd file

If the htpasswd binaray is available `htpasswd -c ./auth/htpasswd username` enter a password at the prompt

### Run Docker Command

Make sure you are at `/mnt/user/docker_registry`

```
docker run -d \
  --restart=always \
  --name registry \
  -v "$(pwd)"/storage:/var/lib/registry \
  -v "$(pwd)"/certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/reg.server.local+1.pem \
  -e REGISTRY_HTTP_TLS_KEY=/certs/reg.server.local+1-key.pem \
  -v "$(pwd)"/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -p 443:443 \
  registry:2
```

### Use

Login: 

`docker login reg.server.local`

Push: 

`docker tag some-image reg.server.local/some-image`

`docker push reg.server.local/some-image`

From your browser visit "https://reg.server.local/v2/_catalog" and the results should look something like this

```
{"repositories":["some-image"]}
```






# docker-setup
A collection of "docker run" commands to setup various apps

The additional files in the repo are instructions on some more indepth setups or other potentially useful material. 


### Ackee - Analytics minus the bad parts

Requires MongoDB

```dockerfile
docker run -p 3000:3000 -e ACKEE_MONGODB='mongodb://db:27017/ackee' -e ACKEE_USERNAME='username' -e ACKEE_PASSWORD='mypass' --name=ackee electerious/ackee
```

Running behind a reverse proxy you'll likely need these headers set

```
proxy_set_header          Access-Control-Allow-Origin "*" always;
proxy_set_header          Access-Control-Allow-Methods "GET, POST, PATCH, OPTIONS" always;
proxy_set_header          Access-Control-Allow-Headers "Content-Type" always;
```

[Github](https://github.com/electerious/Ackee)

[Official Site](https://ackee.electerious.com/)


### Bitwarden Password Manager

```dockerfile
docker run -d --name bitwarden -v /docker/bitwarden:/data -p 8640:80 bitwardenrs/server:latest
```

[Reference](https://hub.docker.com/r/bitwardenrs/server)

### Caddy Webserver 

~~~dockerfile
docker run \
    --name caddy \
    -d \
    -p 58080:8080 \
    -p 80:80 -p 443:443 \
    -v /docker/caddyofficial/Caddyfile:/etc/Caddyfile \
    -v /docker/caddyofficial:/root/.caddy \
    --log-opt "max-size=100m" \
    -e "HTTP_USER=foo" \
    -e "HTTP_PASS=bar" \
    --restart always \
    jpillora/caddy
~~~

### Docker Registry UI

~~~
docker run -d --name=registrywatch -p 8600:8000 -v /docker/regui/config.yml:/opt/config.yml:ro quiq/docker-registry-ui
~~~

[Reference](https://github.com/Quiq/docker-registry-ui)


### Docker Registry 

View a [step-by-step setup](https://github.com/brettjouwstra/docker-setup/blob/main/docker_registry.md)

### Firebird - The Open Source SQL

```shell
docker run -d \
	--name firebird \	
	-p 3050:3050 \
	-v /somehostdir/firebird/backup/:/var/lib/firebird/2.5/backup/ \
	-v /somehostdir/firebird/data/:/var/lib/firebird/2.5/data/ \
almeida/firebird
```
Firebird credentials 
    Username: SYSDBA 
    Password: masterkey 
  
 ```
 //enter container console
$ docker exec -i -t firebird /bin/bash

//restore backup
$ gbak -c -v -user SYSDBA -password masterkey /var/lib/firebird/2.5/backup/dbname.fbk localhost:/var/lib/firebird/2.5/data/dbname.fdb
```

### Portainer - GUI for Docker

```
docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer/data:/data portainer/portainer
```

[Portainer URL](https://portainer.readthedocs.io/en/stable/deployment.html) 

### Lychee - Photo Management

```dockerfile
docker run -d --name=lychee \
  -e DB_HOST=<host-ip> \
  -e DB_USERNAME=username \
  -e DB_PASSWORD=secret \ 
  -e DB_DATABASE=db_name \
  -v /docker/lychee/config:/config
  -v /docker/lychee/pictures:/pictures
   linuxserver/lychee
```

### MongoDB - Database

```
docker run -it \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=<password> \
    -v /docker/mongo/db:/data/db \
    -v /docker/mongo/config:/data/configdb \
    --name mongodb \
    -p 27017:27017 \
    --restart unless-stopped \
    -d mongo
```

[Dockerhub](https://hub.docker.com/_/mongo)

This command is for arm cpus (i.e. a Raspberry Pi)

```Dockerfile
docker run -d \
--name rpi3-mongodb3 \
--restart unless-stopped \
-v /docker/mongo/data/db:/data/db \
-v /docker/mongo/data/configdb:/data/configdb \
-p 27017:27017 \
-p 28017:28017 \
andresvidal/rpi3-mongodb3:latest \
mongod --auth

# Afer, connect to the container 

docker exec -it rpi3-mongodb3 mongo admin

# Create a username/password for your user

db.createUser({ user: "name", pwd: "password", roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] })
```

### NGINX

~~~Dockerfile
docker run -d \
  --name=nginx \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=America/Denver \
  -p 80:80 \
  -p 443:443 \
  -v /docker/nginx/appdata/config:/config \
  --restart unless-stopped \
  ghcr.io/linuxserver/nginx
~~~

### NGINX Proxy Manager

~~~
docker run -d --name=nginx-proxy-manager -p 8181:8181 -p 8080:8080 -p 4443:4443 -v /docker/nginx-proxy-manager:/config:rw jlesage/nginx-proxy-manager
~~~

Default Username/Password:
admin@example.com / changeme

*Note* You'll find it easier to set the port configuration like this `-p 80:8080 -p 443:4443`

[Project Page](https://github.com/jlesage/docker-nginx-proxy-manager)


### Plex Media Server (PMS)

```dockerfile
docker run -d --name=plex \
   --net=host -e PUID=1000 \
   -e PGID=1000 \
   -e VERSION=docker \
   -e UMASK_SET=022 `#optional` \
  -e PLEX_CLAIM= `#optional` \
  -v /mnt/ssd/plex/config:/config \
  -v /mnt/ssd/plex/tv:/tv \ 
  -v /mnt/ssd/plex/movies:/movies \ 
  --restart unless-stopped \
  linuxserver/plex 
```

### Radaar - Movie Management

~~~
docker run -d --name=radarr -e PUID=1000 -e PGID=1000 -e TZ=America/Denver -p 7878:7878 -v /somepath/radaar/data:/config -v /somepath/radaar/movies:/movies -v /somepath/radarr/downloads:/downloads --restart unless-stopped linuxserver/radarr
~~~

### Redis 

~~~
docker run --name my-redis-container -p 7001:6379 -d redis
~~~

### Resilio-Sync (a.k.a BitTorrent Sync)

~~~
docker run -d --name=resilio-sync -e PUID=1000 -e PGID=1000 -e TZ=America/Denver -e UMASK_SET=022 -p 8888:8888 -p 55555:55555 -v /docker/res-sync/config:/config -v /docker/res-sync/downloads:/downloads -v /docker/res-sync/data:/sync --restart unless-stopped linuxserver/resilio-sync
~~~

### Shaarli - Self-hosted Link Sharing

~~~
docker run -d --name=shaarli -p 80:80 -v /docker/shaarli/data:/var/www/shaarli/data -v /docker/shaarli/tpl:/var/www/sharrli/tpl -v /docker/shaarli/assets:/var/www/sharrli/assets shaarli/shaarli
~~~

### Projectsend - Send Files, Expire Links, etc. 

~~~
docker run -d --name=projectsend -e PUID=1000 -e PGID=1000 \
  -e MAX_UPLOAD=5000 -p 8730:80 -v /docker/projsend/config:/config \
  -v /docker/projsend/data:/data --restart unless-stopped linuxserver/projectsend
~~~

[Reference](https://hub.docker.com/r/linuxserver/projectsend/)

### Shiori - Bookmark Management

~~~
docker run -d --name=shiori -p 8080:8080 -v /docker/shiori:/srv/shiori radhifadlillah/shiori
~~~

[Reference](https://hub.docker.com/r/radhifadlillah/shiori)

### Snibox - Code Snippets

~~~
docker run -d --name snibox -v /docker/snibox:/app/db/database -p 3000:3000 --restart always snowmean/snibox-sqlite
~~~

### uLogger - Personal Tracker

```dockerfile
docker run -d \
    -p 7180:80 \
    -e ULOGGER_ENABLE_SETUP=1 \
    -e ULOGGER_DB_HOST=172.18.0.3 \
    -e ULOGGER_DB_USER=dbuser \
    -e ULOGGER_DB_PASSWORD=dbpass \
    -e ULOGGER_LATITUDE=LAT \  # Enter actual coordiantes in these
    -e ULOGGER_LONGITUDE=LONG \
    -e ULOGGER_ADMIN_USER=admin \
    –network docker_default \
    plaguedr/ulogger
```

### Visual Studio Code Server

```dockerfile
docker run -d \
  --name=code-server \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=America/Denver \
  -e PASSWORD=password `#optional` \
  -e SUDO_PASSWORD=password `#optional` \
  -p 8443:8443 \
  -v /docker/vcode/config:/config \
  --restart unless-stopped \
linuxserver/code-server
```

### WebDAV

~~~
docker run --restart always -v /docker/webdav:/var/lib/dav \
    -e AUTH_TYPE=Digest -e USERNAME=user -e PASSWORD=pass \
    --publish 8550:80 -d bytemark/webdav
~~~

[Reference](https://hub.docker.com/r/bytemark/webdav/)


### Webservers (Tiny ones)

```dockerfile
docker run -d --name tinyweb -e "TINYWEB_PORT=9909" -p 80:9909 -v /docker/web:/www cyd01/tinyweb

# or 

docker run --name=mini-cal -p 8180:3000 -v /home/brett/mincal:/app/public:ro -d tobilg/mini-webserver
```

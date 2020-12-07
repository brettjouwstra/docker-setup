# docker-setup
A collection of "docker run" commands to setup various apps

### Docker Registry UI

~~~
docker run -d --name=registrywatch -p 8600:8000 -v /docker/regui/config.yml:/opt/config.yml:ro quiq/docker-registry-ui
~~~

[Reference](https://github.com/Quiq/docker-registry-ui)

### Portainer - GUI for Docker

```
docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v /docker/portainer/data:/data portainer/portainer
```

[Portainer URL](https://portainer.readthedocs.io/en/stable/deployment.html) 

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

### NGINX Proxy Manager

~~~
docker run -d --name=nginx-proxy-manager -p 8181:8181 -p 8080:8080 -p 4443:4443 -v /docker/nginx-proxy-manager:/config:rw jlesage/nginx-proxy-manager
~~~

Default Username/Password:
admin@example.com / changeme

*Note* You'll find it easier to set the port configuration like this `-p 80:8080 -p 443:4443`

[Project Page](https://github.com/jlesage/docker-nginx-proxy-manager)

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


### WebDAV

~~~
docker run --restart always -v /docker/webdav:/var/lib/dav \
    -e AUTH_TYPE=Digest -e USERNAME=user -e PASSWORD=pass \
    --publish 8550:80 -d bytemark/webdav
~~~

[Reference](https://hub.docker.com/r/bytemark/webdav/)
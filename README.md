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
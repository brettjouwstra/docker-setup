# docker-setup
A collection of "docker run" commands to setup various apps


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


### WebDAV

~~~
docker run --restart always -v /docker/webdav:/var/lib/dav \
    -e AUTH_TYPE=Digest -e USERNAME=user -e PASSWORD=pass \
    --publish 8550:80 -d bytemark/webdav
~~~

[Reference](https://hub.docker.com/r/bytemark/webdav/)
### Pulling Images

To pull the most current image from the registry use 

```docker
docker pull <image name>
```

To pull the most current image for all images use

```
docker images |grep -v REPOSITORY|awk '{print $1}'|xargs -L1 docker pull 
```

### Check What Service is Using What Port

```
sudo netstat -tulpn | grep LISTEN
```

# Minio - S3 Compatiable Storage

Generate keys, these will serve as your login to the WebUI as well any authentication performed through scripts or tools like rclone, which we'll see below. 

Here's an example or two of how you might consider generating a secret key. Please DO NOT use these... 

All examples run on macOS Big Sur

```bash
echo $DATE | md5 

:: d489ae8e4cce9c2375f439b17f6dd54
```

The next example requires a file, any one will. I've created one named `cmd.txt`

```bash
shasum -a 256 cmd.txt

:: fb598eb6c55f071157d2dfd09d232cc4dcf8e8d6a6aee7993e3f58c3ac704b0a  cmd.txt
```

Setup the Docker Container

```
docker run -d -p 9600:9000 \
  --name minio \
  -e PUID=4100 \
  -e PGID=997 \
  -v /docker/minio/data:/data \
  -e "MINIO_ACCESS_KEY=<key here>" \
  -e "MINIO_SECRET_KEY=<key here>" \
  minio/minio server /data
```

Use the container via [RClone](https://rclone.org/commands/) by running `rclone config` and following the prompts or by editting/creating the file found at `~/.config/rclone/rclone.conf`

The contents should look similar to this

```
[minio]
type = s3
provider = Minio
env_auth = true
access_key_id = <key from before>
secret_access_key = <key from before>
endpoint = http://your_url.com
acl = private
```

These steps obviously assume you've previously installed RClone. 
# docker-encfs
Decrypt an EncFS-directory with Docker. Based on Alpine Linux (16mb)

## Usage
```
docker run -it --name encfs-decrypt \
   --cap-add SYS_ADMIN \
   --device /dev/fuse \
   --security-opt apparmor:unconfined \
   -e ENCFS_PWD=pwd \ 
   -e ENCFS_OPTS=-o allow_other \
   -v "/path/to/source/directory:/src:shared" \
   -v "/path/to/target/directory:/dest:shared" \
   -v "/path/to/encfs.xml:/config/encfs.xml:ro" \
woelfl/docker-encfs
```
## Requirements
Source directory for the source files (i.e., encrypted):
```
-v "/path/to/source/directory:/src:shared"
```
Target directory for the target files (i.e., decrypted): 
```
-v "/path/to/target/directory:/dest:shared"
```
File containting the EncFS config:
```
-v "/path/to/encfs.xml:/config/encfs.xml:ro"
```

## Optional
Environment variable to specify the encfs password
```
-e ENCFS_PWD=pwd
```

Environment variable to pass options to encfs (such as -o allow_other or --reverse)
```
-e ENCFS_OPTS=-o allow_other -o nonempty --reverse
```


## Docker Compose

Docker compose file:
```
version: '2.1'
services:
  encfs-decrypt:
    container_name: encfs-decrypt
    image: woelfl/docker-encfs
    devices:
      - /dev/fuse
    cap_add:
      - SYS_ADMIN
    security_opt:
      - apparmor:unconfined
    environment:
      - ENCFS_PWD=pwd
      - ENCFS_OPTS=-o allow_other,nonempty
    volumes:
      - /path/to/source/directory:/src:shared
      - /path/to/target/directory:/dest:shared
      - /path/to/encfs.xml:/config/encfs.xml:ro

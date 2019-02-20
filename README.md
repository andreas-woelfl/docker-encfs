# docker-encfs
Decrypt an EncFS-directory with Docker. Based on Alpine Linux (16mb)

## Usage
```
docker run -it --name encfs-decrypt \
   --cap-add SYS_ADMIN \
   --device /dev/fuse \
   --security-opt apparmor:unconfined \
   -v "/path/to/source/directory:/src:shared" \
   -v "/path/to/target/directory:/dest:shared" \
   -v "/path/to/password/file:/config/passwd:ro" \
   -v "/path/to/encfs.xml:/config/encfs.xml:ro" \
woelfl/docker-encfs
```
## Requirements
Source directory for the encrypted files:
```
-v "/path/to/source/directory:/src:shared"
```
Target directory for the decrypted files: 
```
-v "/path/to/target/directory:/dest:shared"
```
File containting the EncFS password:
```
-v "/path/to/password/file:/config/passwd:ro"
```
File containting the EncFS config:
```
-v "/path/to/encfs.xml:/config/encfs.xml:ro"
```
## Docker Compose

Exemplary docker compose file:
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
    volumes:
      - /path/to/source/directory:/src:shared
      - /path/to/target/directory:/dest:shared
      - /path/to/password/file:/config/passwd:ro
      - /path/to/encfs.xml:/config/encfs.xml:ro

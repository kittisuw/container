# Basic command
### Running container   
จาก image:redis version ล่าสุด แบบ detach mode (pull imager และ run ในเวลาเดียวกัน)
```
$ docker run -d redis
```
### ตรวจสอบ container ที่ run
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
5294e499ff16        redis               "docker-entrypoint..."   2 minutes ago       Up 28 seconds       6379/tcp            modest_ritchie
```
*** remove all continer :docker rm $(docker ps -aq)
### Stop โดยใช้ prefix image id
```
$ docker stop 52
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
```
### ตรวจสอบ Container ทั้งหมดทั้ง running และไม่ running
```
docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
5294e499ff16        redis               "docker-entrypoint..."   3 minutes ago       Exited (0) 30 seconds ago                       modest_ritchie
```
### Start container ที่เคย stop ไป
```
$ docker start 52
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
5294e499ff16        redis               "docker-entrypoint..."   4 minutes ago       Up 21 seconds       6379/tcp            modest_ritchie
```
### Run container จาก imager redis V.ล่าสุด และ V.4.0 ในเวลาเดียวกัน
```
$ docker run -d redis
$ docker run -d redis:4.0
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
e9f0b7607a64        redis:4.0           "docker-entrypoint..."   6 seconds ago       Up 3 seconds        6379/tcp            heuristic_newton
5294e499ff16        redis               "docker-entrypoint..."   6 minutes ago       Up 2 minutes        6379/tcp            modest_ritchie
```
### Run container ด้วย image:redis V.ล่าสุด และ V.4.0 แบบกำหนด port เพื่ออนุญาติสามารถติดต่อ container ได้จากนอก host ที่ run docker 
```
docker run -d -p6000:6379 redis
bfd19ad28faf1834560eeed2f4a21158a704c40a8701f24981fb427835294d0d

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
bfd19ad28faf        redis               "docker-entrypoint..."   9 seconds ago       Up 3 seconds        0.0.0.0:6000->6379/tcp   wonderful_nightingale

$ docker run -d -p6000:6379 redis:4.0
98210053988efe3610925ec14472f83bdc9e29d79dd19a77f972e1db7c6ebe11
/usr/bin/docker-current: Error response from daemon: driver failed programming external connectivity on endpoint adoring_goldberg (0a9afb1c078bc9d1f3acb15a000f8113ee660e3516aca37319d0c93239ff092d): Bind for 0.0.0.0:6000 failed: port is already allocated.

$ docker run -d -p6001:6379 redis:4.0
6aef4720b618400cffaccc206e9f302aebdde7737a5b71406edcaf5b37ab8157

[kittisuw@thanos-demo ~]$
[kittisuw@thanos-demo ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
6aef4720b618        redis:4.0           "docker-entrypoint..."   10 seconds ago      Up 3 seconds        0.0.0.0:6001->6379/tcp   eager_tesla
bfd19ad28faf        redis               "docker-entrypoint..."   3 minutes ago       Up 3 minutes        0.0.0.0:6000->6379/tcp   wonderful_nightingale
```

### run container map หลาย port
```
$ docker run -d -p3000:80 -p8080:80 --name jib nginx:latest
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                                        NAMES
9423b308b892        nginx:latest        "/docker-entrypoin..."   About a minute ago   Up About a minute   0.0.0.0:3000->80/tcp, 0.0.0.0:8080->80/tcp   jib
[kittisuw@thanos-demo ~]$
```

### ตรวจสอบ image ที่เคยทำการ pull มาที่ local host 
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/redis     latest              bd571e6529f3        9 days ago          104 MB
docker.io/redis     4.0                 191c4017dcdd        6 months ago        89.3 MB
```
# Advance
### log 
```
docker logs -f 6a
1:C 23 Oct 11:19:32.305 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 23 Oct 11:19:32.529 # Redis version=4.0.14, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 23 Oct 11:19:32.529 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 23 Oct 11:19:32.533 * Running mode=standalone, port=6379.
1:M 23 Oct 11:19:32.534 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 23 Oct 11:19:32.534 # Server initialized
1:M 23 Oct 11:19:32.535 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 23 Oct 11:19:32.535 * Ready to accept connections
^C
[kittisuw@thanos-demo ~]$ docker logs eager_tesla
1:C 23 Oct 11:19:32.305 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 23 Oct 11:19:32.529 # Redis version=4.0.14, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 23 Oct 11:19:32.529 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 23 Oct 11:19:32.533 * Running mode=standalone, port=6379.
1:M 23 Oct 11:19:32.534 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 23 Oct 11:19:32.534 # Server initialized
1:M 23 Oct 11:19:32.535 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
```
### run container แบบกำหนดชื่อ container
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
6aef4720b618        redis:4.0           "docker-entrypoint..."   57 minutes ago      Up 57 minutes       0.0.0.0:6001->6379/tcp   eager_tesla
bfd19ad28faf        redis               "docker-entrypoint..."   About an hour ago   Up About an hour    0.0.0.0:6000->6379/tcp   wonderful_nightingale
$ docker stop 6a
6a
$ docker run -d -p6001:6379 --name redis-older redis:4.0
b15c69765f466a45b3213060f5c5f3ede030cfbe878622a62a4f50cb7f752d5c

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
b15c69765f46        redis:4.0           "docker-entrypoint..."   13 seconds ago      Up 4 seconds        0.0.0.0:6001->6379/tcp   redis-older
bfd19ad28faf        redis               "docker-entrypoint..."   About an hour ago   Up About an hour    0.0.0.0:6000->6379/tcp   wonderful_nightingale

$ docker stop bf
bf
$ docker run -d -p6000:6379 --name redis-latest redis
a607858c478cfd7429f3b65521734315927fa61ebea56a7b4ce40ec45ac26b5f
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
a607858c478c        redis               "docker-entrypoint..."   10 seconds ago      Up 2 seconds        0.0.0.0:6000->6379/tcp   redis-latest
b15c69765f46        redis:4.0           "docker-entrypoint..."   5 minutes ago       Up 5 minutes        0.0.0.0:6001->6379/tcp   redis-older
```
### Force delete all container running
* option a is show all containers, q is return only contianer id
```
docker rm -f $(docker ps -aq)
```
## Stop container แบบอ้างอิงชื่อ
```
docker run -d -p3000:80 -p8080:80 --name website nginx:latest
4fe82e175efbed049ab4d40fc52dcc4bdaed71b99e165f1a2b25eecc98031b0f
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                        NAMES
4fe82e175efb        nginx:latest        "/docker-entrypoin..."   2 minutes ago       Up 2 minutes        0.0.0.0:3000->80/tcp, 0.0.0.0:8080->80/tcp   website
$ docker stop website
website
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$
```
### Mapping volume
** ro:read only inside container, rw:read write insite container
** Disable SE LINUX before (https://linuxize.com/post/how-to-disable-selinux-on-centos-8/)
```
docker run --name website -v $(pwd):/usr/share/nginx/html:ro -p8080:80 -d nginx 
```

## access inside container
```
docker exec -it website bash
```

### Share voulume
```
docker run --name website-copy --volumes-from website -d -p8081:80 -d nginx
```
------------------------------------------------------------------------------------------------------
### Docker file with node.js xxx
```
$ sudo yum install npm
$ npm --version
$ npm install --save express
$ vi index.js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.json([{
    name: 'Bob',
    email: 'Bob@gmail.com'
}]))

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})

$ node index.js

$vi Dockerfile
FROM node:latest
WORKDIR /app
ADD . .
RUN npm install
CMD node index.js

$ docker build -t user-service-api:latest . 
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
user-service-api    latest              c9c35db84b4c        34 minutes ago      944 MB

$ docker run -d -p3000:3000 user-service-api:latest
$ docker ps
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
3067bfeff11a        user-service-api:latest   "docker-entrypoint..."   29 minutes ago      Up 29 minutes       0.0.0.0:3000->3000/tcp   frosty_fermi
```




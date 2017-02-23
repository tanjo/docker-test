# docker-test

docker を使ってみた

## Setup

Homebrew で入れてないけど

```sh
brew install virtualbox
```

Docker と Machine

```sh
brew install docker
brew install docker-machine
```

## Version

```sh
> virtualbox --help                             
Oracle VM VirtualBox Manager 5.0.26
> docker -v
Docker version 1.13.1, build 092cba3
> docker-machine -v
docker-machine version 0.9.0, build 15fd4c7
```

## Status

```sh
> docker-machine ls
NAME   ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER    ERRORS
dev    -        virtualbox   Running   tcp://192.168.99.100:2376           v1.13.1   

> docker-machine status dev
Running
```

## SSH

```
> docker-machine ssh dev
##         .
## ## ##        ==
## ## ## ## ##    ===
/"""""""""""""""""\___/ ===
~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
\______ o           __/
\    \         __/
\____\_______/
_                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.13.1, build HEAD : b7f6033 - Wed Feb  8 20:31:48 UTC 2017
Docker version 1.13.1, build 092cba3
docker@dev:~$
```

## Image

- [Explore - Docker Hub](https://hub.docker.com/explore/)

### Pull CentOS

```sh
docker@dev:~$ docker pull centos:7
7: Pulling from library/centos
45a2e645736c: Pull complete
Digest: sha256:c577af3197aacedf79c5a204cd7f493c8e07ffbce7f88f7600bf19c688c38799
Status: Downloaded newer image for centos:7
```

### Check images

```sh
docker@dev:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              7                   67591570dd29        2 months ago        192 MB
```

## Create container

```sh
docker@dev:~$ docker create -it --name centos7 centos:7
a1049214463be37818a160a37b0b233c744217cf5b016b6a99434256e5e0241a
```

or

```sh
docker@dev:~$ docker run -it --detach --name centos7.01 centos:7
005a4e5371968d420ffccf5d89935e767714b71f2155c40fb557eb0bdbfe207b
```

### Check container

```sh
docker@dev:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
005a4e537196        centos:7            "/bin/bash"         31 seconds ago       Up 30 seconds                           centos7.01
a1049214463b        centos:7            "/bin/bash"         About a minute ago   Created                                 centos7
```

### Delete container

```sh
docker@dev:~$ docker rm centos7
centos7
```

### Stop container

```sh
docker@dev:~$ docker stop centos7.01
centos7.01
```

### Delete container

```sh
docker@dev:~$ docker rm `docker ps -a -q`
005a4e537196
```

### Create and Attach container

```sh
docker@dev:~$ docker run -it --hostname hoge centos:7
[root@hoge /]#
```

### Bye

```sh
[root@hoge /]# exit
exit
```

### Check container

```sh
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
c82711d6a085        centos:7            "/bin/bash"         3 minutes ago       Exited (0) 47 seconds ago                       flamboyant_hugle
```

### Attach container

```sh
docker@dev:~$ docker start c82711d6a085
c82711d6a085
docker@dev:~$ docker attach c82711d6a085
[root@hoge /]#
```

### Exec

```sh
docker@dev:~$ docker exec -it flamboyant_hugle ls
anaconda-post.log  dev	home  lib64	  media  opt   root  sbin  sys	usr
bin		   etc	lib   lost+found  mnt	 proc  run   srv   tmp	var
```

#### Multi

```sh
docker@dev:~$ docker exec -it flamboyant_hugle /bin/bash
[root@hoge /]# hostname
hoge
[root@hoge /]#
```

## End

```
[root@hoge /]# exit
exit
docker@dev:~$ exit
```

## Kill

```
> docker-machine kill dev                                                                                                                                                      (git)-[master]
Killing "dev"...
Machine "dev" was killed.
```

## Remove

```
> docker-machine rm dev
About to remove dev
WARNING: This action will delete both local reference and remote instance.
Are you sure? (y/n): y
Successfully removed dev

> docker-machine ls
NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS
```

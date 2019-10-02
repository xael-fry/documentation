# Docker 
 
## Windows

Docker use a default VM that contains the containers. By default the VM shared the folder \\?\c:\Users as c/Users, so you may need to add a folder if you want put the code in another folder Example E:/devel

```batch   
docker-machine stop default
VBoxManage sharedfolder add default --name "e/devel" --hostpath "E:\devel" 
VBoxManage showvminfo default | grep -i share -A4
docker-machine start default
```

## Operations on containers

### Stop / remove all Docker containers

One liner to stop / remove all of Docker containers:

```batch 
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```


```batch 
ec2-user@ip-10-196-21-20 main]$ cat Dockerfile
FROM ubuntu:16.04

VOLUME /project_w

RUN apt-get update && apt-get install -y \
   openjdk-8-jdk \
   maven \
   software-properties-common \
   unzip \
   curl \
   xvfb \
   libxi6 \
   libgconf-2-4

WORKDIR /project_w/projet/test/

ENTRYPOINT ["mvn", "clean", "verify"]
```

Build the containers
```
docker build -t ach:latest . > /dev/null
docker run -it  --rm -v $(pwd):/project -t ach:latest > /dev/null
```

vagrant@docker:/etc$ docker run  --user 1001 -it  --rm -v $(pwd):/project_w -v $HOME/.m2:/root/.m2 -t ach:latest


Define Proxy settings
```
cd .docker/
   51  touch config.json
   52  nano config.json
```

```
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://frigo.mediametrie.fr:3128",
     "httpsProxy": "http://frigo.mediametrie.fr:3128",
     "noProxy": "*.test.example.com,.example2.com"
   }
 }
}
```

Define zcaler  certificates
```
sudo cp /vagrant/host/ZscalerRootCA.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
sudo service docker restart
```
 
 


Enable userns-remap on the daemon

You can start dockerd with the --userns-remap flag or follow this procedure to configure the daemon using the daemon.json configuration file. The daemon.json method is recommended. If you use the flag, use the following command as a model:

```
$ dockerd --userns-remap="testuser:testuser"

    Edit /etc/docker/daemon.json
        
   30  sudo systemctl daemon-reload
   31  sudo systemctl show --property Environment docker
   32  sudo systemctl restart docker
   
> sudo touch /etc/docker/daemon.json
sudo nano /etc/docker/daemon.json
```

Enable user namespaces in /etc/default/docker:

```
DOCKER_OPTS="--userns-remap=ns1"
```

Restart daemon service docker restart, ensure /var/lib/docker/500000.500000 directory is created.


docker build --build-arg UID=$UID -t ach:latest . > /dev/null



----------------------------

````
FROM ubuntu:latest

ENV http_proxy "http://frigo.mediametrie.fr:3128"

VOLUME /project_w

# Defines argument which can be passed during build time.
ARG UNAME=docker
ARG UID=1000
ARG GID=1000

USER root

RUN apt-get update && apt-get install -y \
   openjdk-8-jdk \
   maven \
   software-properties-common \
   unzip \
   curl \
   xvfb \
   libxi6 \
   libgconf-2-4


RUN groupadd -g $GID $UNAME
RUN useradd -d /home/$UNAME -m -u $UID -g $GID -s /bin/bash $UNAME

#RUN useradd -d /home/docker -ms /bin/bash -g root -G $GID -u $UID docker

USER $UNAME

RUN id

WORKDIR /project_w/projet/test/

ENTRYPOINT [ "mvn", "clean", "verify"]
```
----------------------------


 docker build --build-arg UID=$(id -u) --build-arg UNAME=docker --build-arg GID=$(id -g) -t ach:latest . > /dev/null
 
 
 docker run --user docker:docker -it  --rm -v $(pwd):/project_w -v $HOME/.m2:/home/docker/.m2 -t ach:latest
 
 
``` 
 
------------------------------------------------------
docker run  -u 1000:1000 -it  --rm -v $(pwd):/project_w -v $HOME/.m2:/root/.m2 -t ach:latest


docker build -t test:latest . > /dev/null



docker build --build-arg UID=$(id -u) --build-arg GID=$(id -g) -t test:latest




docker build --build-arg UID=$(id -u) --build-arg GID=$(id -g) -t test:latest --file python3.Dockerfile  .


docker run  -u 1000:1000 -it  --rm  -t test:latest


 docker run -t -d -u 1000:1000 -w /sample_module -v /workspace/sample_module:/sample_module:rw,z -t test:latest
 
 t -d
 -it  --rm
 
  docker run -it  --rm -u 1000:1000 -w /sample_module -v /workspace/sample_module:/sample_module:rw,z -t test:latest




  docker run -it  --rm -u 1000:1000 -w /sample_module -v /workspace/sample_module:/sample_module:rw,z -t test:latest sh -c 'cd jenkinsfile_docker_python/sample_module && pip install -r requirements.txt && mkdir -p nosetests-reports && nosetests tests --with-xunit --xunit-file=nosetests-reports/nosetests-py3.xml'





   
   
   



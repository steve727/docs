### install dockeer engine & docker-compose on debian
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo groupadd docker
sudo usermod -aG docker steve
newgrp docker
docker run hello-world
```
### install docker (CentOS)
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum -y install docker-ce
sudo systemctl enable --now docker
sudo usermod -aG docker cloud_user
docker run hello-world
```
### show containers and images
```
docker ps -a
docker images -a
```
### container commands
```
docker container ls
docker container exec -it [container name] /bin/bash
```
### prune /var/lib/docker/overlay2
```
du -sch /var/lib/docker/overlay2/*
docker system prune -a -f
docker system prune --all --volumes --force
```
### delete logs
`find /var/lib/docker/containers/ -type f -name “*.log” -delete`

[zabbix](https://www.zabbix.com/documentation/current/manual/installation/containers)

### images
`docker images ls`
### remove volumes
`docker volume rm $(docker volume ls -qf dangling=true)`

### docker repo for rhel
```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```
### ubuntu
`docker run -i -t ubuntu /bin/bash`
    
### push repo
```
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname
docker push steve727/devcontainer:tagname
```    
### docker [nginx](https://hub.docker.com/_/nginx) - http://localhost:8080/
```
docker pull nginx
docker run -it --rm -d -p 8080:80 --name web nginx
docker stop web
docker rm nginx
docker ps -a
```
### mount local content directory to nginx
```
/usr/share/nginx/html
    
mkdir ~/site-content
vi index.html
  <!doctype html>
  <html lang="en">
  <head>
  <meta charset="utf-8">
  <title>Docker Nginx</title>
  </head>
  <body>
  <h2>Hello from Nginx container</h2>
  </body>
  </html>
    
docker run -it --rm -d -p 8080:80 --name web -v ~/site-content:/usr/share/nginx/html nginx
    
docker stop web
    
vi Dockerfile
  FROM nginx:latest
  COPY ./index.html /usr/share/nginx/html/index.html
            
docker build -t webserver .
docker run -it --rm -d -p 8080:80 --name web webserver
``` 
### grafana
`docker run -d --name=grafana -p 3000:3000 grafana/grafana`
    
`docker stop grafana`
    
### nextcloud
`docker pull nextcloud`

`docker run -d -p 8080:80 nextcloud`
  
### [httpd](https://hub.docker.com/_/httpd)
```
docker pull httpd:2.4
docker run --name webtemplate -d httpd:2.4
docker ps    
docker exec -it webtemplate bash
apt update && apt install git -y
git clone  https://github.com/linuxacademy/content-widget-factory-inc.git /tmp/widget-factory-inc
ls -l /tmp/widget-factory-inc/
ls -l htdocs/
rm htdocs/index.html
cp -r /tmp/widget-factory-inc/web/* htdocs/
ls -l htdocs/
exit
docker ps
docker commit <CONTAINER_ID> example/widgetfactory:v1
docker images
docker exec -it webtemplate bash
rm -rf /tmp/widget-factory-inc/
apt remove git -y && apt autoremove -y && apt clean 
docker ps
docker commit <CONTAINER_ID> example/widgetfactory:v2
docker images
docker rmi example/widgetfactory:v1
docker run -d --name web1 -p 8081:80 example/widgetfactory:v2
docker run -d --name web2 -p 8082:80 example/widgetfactory:v2
docker run -d --name web3 -p 8083:80 example/widgetfactory:v2
docker ps
docker stop webtemplate
```   
### docker storage
    docker images
    docker run -d --name db1 postgres:12.1
    docker run -d --name db2 postgres:12.1
    docker ps
    docker volume ls
    docker inspect db1 -f '{{ json .Mounts }}' | python -m json.tool
    docker inspect db2 -f '{{ json .Mounts}}' | python -m json.tool
    docker run -d --rm --name dbTmp postgres:12.1    
    docker ps -a
    docker volume ls
    docker stop db2 dbTmp
    docker volume ls    
    docker ps -a
    docker volume create website
    docker volume ls
    sudo cp -r /home/cloud_user/widget-factory-inc/web/* /var/lib/docker/volumes/website/_data/    
    sudo ls -l /var/lib/docker/volumes/website/_data/
    docker run -d --name web1 -p 80:80 -v website:/usr/local/apache2/htdocs:ro httpd:2.4    
    docker ps
    docker run -d --name webTmp --rm -v website:/usr/local/apache2/htdocs:ro httpd:2.4
    docker ps
    docker stop webTmp
    docker ps -a
    docker volume prune
    docker ps -a
    docker rm db2
    docker volume prune
    docker volume ls
    
### backup & restore the docker volume
    sudo -i
    docker volume inspect website
    tar czf /tmp/website_$(date +%Y-%m-%d-%H%M).tgz -C /var/lib/docker/volumes/website/_data .
    ls -l /tmp/website_*.tgz
    exit
    docker run -it --rm -v website:/website -v /tmp:/backup bash tar czf /backup/website_$(date +%Y-%m-%d-%H-%M).tgz -C /website .
    ls -l /tmp/website_*.tgz
    sudo -i
    cd /var/lib/docker/volumes/
    ls -l
    cd website/_data/
    rm * -rf
    ls -l /tmp/website_*.tgz
    tar xf <BACKUP_FILE_NAME>.tgz .
    /tmp/website_2021-03-22-16-23.tgz
    ls -l

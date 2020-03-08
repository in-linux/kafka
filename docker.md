Run docker container with multiple port mapping and terminal interactive:
```
docker run -it -p 2181:2181 -p 3030:3030 -p 8081:8081 -p 8082:8082 -p 8083:8083 -p 9092:9092 -p 9581:9581 -p 9582:9582 -p 9583:9583 -p 9584:9584 -e ADV_HOST=127.0.0.1 repository/image-name:latest
```
Run container with ghost blogging platform
```
docker run -d --name container-name -e url=http://localhost:3001 -p 3001:2368 ghost
```
How to install docker engine
```
https://docs.docker.com/install/linux/docker-ce/ubuntu/
https://docs.docker.com/install/linux/docker-ce/centos/
```

How to install docker compose

```
https://docs.docker.com/compose/install/
```

How to install docker compose alternative method

```shell
sudo apt install -y docker;
sudo usermod -aG docker ec2-user;
sudo service docker start;
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose;
chmod +x /usr/local/bin/docker-compose;
docker-compose --version
```

Uninstall the Docker CE:

```shell
sudo apt-get purge docker-ce
```
Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

```shell
sudo rm -rf /var/lib/docker
You must delete any edited configuration files manually.
```

Docker compose file = config file (obrazy ISO systemów które mają być uruchomione)

```
docker-compose.yaml
```
Display all containers running and stoped {a = all}
```
docker ps -a
```
Display last used container
```
docker ps -al
```

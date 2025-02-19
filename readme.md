
## YouTube Link
For the full 1 hour course watch on youtube:
https://www.youtube.com/watch?v=6YZvp2GwT0A

# Installation
## Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)
```
sudo docker build -t myjenkins-blueocean:2.426.2 .
```
#IF you are having problems building the image yourself, you can pull from my registry (It is version 2.332.3-1 though, the original from the video)
```
docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1
```

## Create the network 'jenkins'
```
sudo docker network create jenkins
```
to see all networks:
```
sudo docker network ls
```

## Run the Container
### MacOS / Linux
```
sudo docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.426.2
```

### Windows
```
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.426.2
```


## Get the Password
```
sudo docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

## Connect to the Jenkins
```
https://localhost:8080/
```
# Remove docker container:

  List all running containers:
```
sudo docker ps
```
  Stop the running container:
```
sudo docker stop <container_id_or_name>
```
```
sudo docker stop fe6b2c61496b
```
  Remove the container:
```
sudo docker rm <container_id_or_name>
```
```
sudo docker rm fe6b2c61496b
```
  Remove the Docker image:
```
sudo docker rmi <container_id_or_name>
```
```
sudo docker rmi -f <container_id_or_name>
```
```
sudo docker rmi fe6b2c61496b
```
```
sudo docker rmi -f fe6b2c61496b
```

## Installation Reference:
https://www.jenkins.io/doc/book/installing/docker/


## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine

https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
```
sudo docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
sudo docker inspect <container_id> | grep IPAddress
```

## Using my Jenkins Python Agent
```
docker pull devopsjourney1/myjenkinsagents:python
```

# Build, test, and deploy Docker applications with Kubernetes

## Dive into Docker


![image](https://user-images.githubusercontent.com/104793540/210808264-f792210e-f126-4b52-8812-d7b5ffadde63.png)
![image](https://user-images.githubusercontent.com/104793540/210808659-48121796-b886-4593-9f9d-fd8c446c084e.png)
![image](https://user-images.githubusercontent.com/104793540/210808389-f8015179-00dc-404f-8b07-ef3658d78301.png)
![image](https://user-images.githubusercontent.com/104793540/210808886-f2cfb497-e194-403a-b525-12b809da6124.png)


- started docker desktop 
- `docker run hello-world`

Steps to go through in detail: 

 1. The Docker client contacted the Docker daemon.
    show how
 
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
    show how 
 
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
    show how 
 
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
    show how 

### image to container behind the scenes:

- a container is a running process along with a subset of physcial resources that are allocated to that process specifically.  
- In a container a portion of each resource is made available to process  by the kernal

![image](https://user-images.githubusercontent.com/104793540/211091326-d29cd76d-caaa-40a6-a400-5596e9a9109b.png)

- an image is a file snapshot and place inside segment of HD i.e hd with just chrome and python installed as shown above. 
- running process = startup command i.e run chrome for me

on my downloaded docker in windows the following is happening:

![image](https://user-images.githubusercontent.com/104793540/211092588-da64949b-e98f-47f8-9b08-a6c148599f9e.png)

check linux virtual machine with `docker version` to confirm:

![image](https://user-images.githubusercontent.com/104793540/211093016-5e5dfd48-4c7a-4dc3-8cd0-e13a9fc8e619.png)


## Manipulating Containers with the Docker client 

overwritting default commands 
![image](https://user-images.githubusercontent.com/104793540/211093842-152d96f0-d3cf-472e-a03b-47eea97cb9c3.png)

- `docker run busybox echo hi there ayan`
- `docker run busybox ls` - check folders inside container 
- `docker run -d redis`
- `docker ps` list all running containers 
- `docker stop id`

docker run = docker create + docker start -a id
redis - in memory data store, commonly used with web application. Redis is an open source, in-memory data structure store, used as a database, cache, and message broker. MongoDB and Redis are modern NoSQL databases.

- `docker run redis` (redis-server)(must run cli inside the container where redis server is)
- **execute command inside running container** `docker exec -it container id command` > `docker exec -it a44125e3f27d redis-cli `

IT flags allows for input of terminal cmd into container linux env and get info back out to terminal 

shell access/ terminal access in running container: `docker exec -it id sh` > type commands for unix env - FULL TERMINAL ACCESS (ctrl c or exit or d/ctrl d)

starting with shell: `docker run -it busybox sh`

## Building custom images through Docker Server
Dockerfile > docker client > docker server > usable image

![image](https://user-images.githubusercontent.com/104793540/211355736-3446da1e-153a-470d-b19a-7612656a7ca0.png)

https://docs.docker.com/engine/reference/builder/

````
FROM
to pull base image 

RUN
to execute commands 

CMD
to provide defaults for an executing container 

EXPOSE
informs docker that the container listens on the speficied network ports at runtime 

ENTRYPOINT
to configure a container that will run as an executable

WORKDIR
sets the working diretory 

COPY
copy a directory from your local machine to docker container 

ADD
copy files and folders from you local machine to docker containers 

ENV 
to set environmental variables 

````

- `docker build -t dirofdockerfile`   or `docker build .`     `docker run id`

![image](https://user-images.githubusercontent.com/104793540/211359527-931ce5f4-3e3e-4b56-bc10-c2269d6b5c4d.png)

tagging: `docker build -t asalad42/repoorprojectname: latest .`  `docker run asalad42/repoorprojectname`
## Making Real Projects with Docker 

![image](https://user-images.githubusercontent.com/104793540/211370912-a85ec574-5844-44e0-88f5-44aa4ba49abb.png)

![image](https://user-images.githubusercontent.com/104793540/210812020-e9adef8a-195f-4408-92bb-220ef871e7d0.png)

![image](https://user-images.githubusercontent.com/104793540/211372595-789e5775-8826-4cd4-9922-767590ba4804.png)

port mapping:

![image](https://user-images.githubusercontent.com/104793540/211580416-47ca7a35-8d29-4670-9a25-056397b5e83a.png)
![image](https://user-images.githubusercontent.com/104793540/211580218-9fecdfc9-a498-4780-bdb9-fcf37b01cff2.png)


- `docker build -t asalad42/simpleweb .`
- `docker run -p 5000:8080 asalad42/simpleweb`   i.e localhost:5000
- if i want to change port in container must do so in application 

![image](https://user-images.githubusercontent.com/104793540/211650312-59c9ec4d-0c56-467f-ab64-0a7172e1d7f1.png)

Specifying the WORKDIR:
- `docker run -it asalad42/simpleweb sh` see with `ls`
- WORKDIR /usr/app
- any following command will be executed relative to this path in the container
- rebuild after with `docker build -t asalad42/simpleweb .`

## Docker Compose with Multiple Local Containers 

Goal for number of visits webpage project: 

![image](https://user-images.githubusercontent.com/104793540/211883515-e1ad334c-0ea1-4bb2-a9ca-b22a45f3c8cc.png)

Use docker compose:
- separate CLI that gets installed along with Docker
- **used to startup multiple docker containers at the same time** 
- automates some of the long winded arguments passing to docker run 

Flow: docker-compose.yml > docker-compose CLI 

containers i want to create:
1. redis-server: make using redis image
2. node-app: make using dockerfile  and map port 8081 to 8081 
3. networking between containers automatically established via docker compose

- `docker-compose up`  (docker run myimage)
- `docker-compose up --build`   (docker build. & docker run myimage)
- `docker-compose up -d` launch in background
- `docker compose down` stop containers

## Production-Grade Workflow 

## CI and Deployment 

## Building Multi-Container Application 

## Continous Integration workflow for multiple images 

## Multi-Container Deployment to AWS 

## Maintaing sets of containers with Deployments

## Multi-Container App with Kubernetes 

## Hnadling Traffic with Ingress Controllers 

## Kubernetes Production Depployment 

## HTTPS setup with Kubernetes

## Local Deployment with Skaffold 

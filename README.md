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
- `docker ps` list all running containers 


## Building custom images through Docker Server

## Making Real Projects with Docker 

![image](https://user-images.githubusercontent.com/104793540/210812020-e9adef8a-195f-4408-92bb-220ef871e7d0.png)

## Docker Compose with Multiple Local Containers 

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

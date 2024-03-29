# Build, test, and deploy Docker applications 

## Docker Framework


![image](https://user-images.githubusercontent.com/104793540/210808264-f792210e-f126-4b52-8812-d7b5ffadde63.png)
![image](https://user-images.githubusercontent.com/104793540/210808659-48121796-b886-4593-9f9d-fd8c446c084e.png)
![image](https://user-images.githubusercontent.com/104793540/210808389-f8015179-00dc-404f-8b07-ef3658d78301.png)
![image](https://user-images.githubusercontent.com/104793540/210808886-f2cfb497-e194-403a-b525-12b809da6124.png)


- started docker desktop 
- `docker run hello-world`


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
- `docker create busybox`
- `docker start containerid`
- `docker ps` list all running containers 
- `docker ps -a`
- `docker start -a id`
- `docker stop id`
- `docker kill containerid`
- `docker system prune`
- `docker logs containerid` good for debugging
- `docker inspect` returns low-level information on Docker objects
- `wsl --unregister docker-desktop`
- `wsl --unregister docker-desktop-data`
- `wsl --terminate docker-desktop`
- `wsl --terminate docker-desktop-data`
- Navigate to Appdata/Roaming/Docker folder in your user account
- Open the settings file(JSON file) and make sure the following are set this way :point_down:

"integratedWslDistros" : [ ]
"enableIntegrationWithDefaultWslDistro" : false,

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

networking between containers automatically established via docker compose

- `docker-compose up`  (docker run myimage)
- `docker-compose up --build`   (docker build. & docker run myimage) (rebuild container)
- `docker-compose up -d` launch in background
- `docker-compose down` stop containers
- restart: always (restart policies for container maintenance and automatic container restart) (always keep server running)
- `docker-compose ps` status of two running containers 

## Production-Grade Workflow (Single React Application backedup by an Nginx Server)

using docker in production environment

![image](https://user-images.githubusercontent.com/104793540/212204670-7771928f-7acc-4f27-8999-f9422ccc7285.png)

Prerequisites: 
1. Node needed for project: https://nodejs.org/en/download/  after installation, check with `node -v`
2. Create React App globally and generate the application:
- https://create-react-app.dev/docs/getting-started/#npx
- `npx create-react-app frontend`

![image](https://user-images.githubusercontent.com/104793540/212315815-9d9c02a0-b05f-49b0-894b-a3b95dce10e5.png)

- `npm run start` starts up development server. for dev use only
- `npm run test` runs tests asscociated with project
- `npm run build` build a **production** version of the application 


## Dev

![image](https://user-images.githubusercontent.com/104793540/214830536-76bb069d-965c-455f-9c19-406b3a6a1135.png)

### Option 1 

#### Part 1.1 Creating the Dev Dockerfile
Dockerfile.dev in development 
- build image with `docker build -f Dockerfile.dev .` using f for specifying file to use build from

#### Part 1.2 Starting the container 

- `docker run -p 3000:3000 sha256:12ff0233183d8dabb5043917f0cbfc757cd8`
![image](https://user-images.githubusercontent.com/104793540/212553786-5ae06125-4db5-44f0-8362-7539398598cf.png)
![image](https://user-images.githubusercontent.com/104793540/212553681-8c1feb25-888c-483a-b08a-d7eb56e3f44b.png)

- `docker build -t asalad42/docker-react -f Dockerfile.dev .`
- `docker run -d -p 3000:3000 asalad42/docker-react`
- `docker push asalad42/docker-react:latest`

#### Part 1.3 Ensuring changes to source code automatically affect container 

- problem: made changes in App.js but not seen in browser 
- initially copied everything with COPY .. command in Dockerfile.dev
- want to avoid stopping and restarting container, how? **Make use of Docker Volumes**

![image](https://user-images.githubusercontent.com/104793540/212554428-2d618542-0e67-46f6-8e89-a86dd547d243.png)
![image](https://user-images.githubusercontent.com/104793540/212554446-2d626de6-11ba-4f32-8223-67054e4c554a.png)
- first switch: bookmarks node folder in container so the second switch does not overwrite and delete udring its mapping
- second switch (note colon for mapping into app folder in container
 ![image](https://user-images.githubusercontent.com/104793540/212555719-3e97dc76-ff17-4f1b-9f0d-6be4f73da766.png)
 
### Option 2 (Best Practice)(running all docker commands within WSL)

- open wsl and run `cd ~` `pwd`  should be in /home/username
- `explorer.exe .`  and move the frontend project directory into the WSL file browser window:
- Using the WSL terminal build your Docker image as you typically would `docker build -f Dockerfile.dev -t ayan:frontend .`
- Using ~ alias in WSL terminal to start and run a container: `docker run -it -p 3000:3000 -v /home/node/app/node_modules -v ~/frontend:/home/node/app ayan:frontend`
- **Easier to use** docker-compose up than this long run command

![image](https://user-images.githubusercontent.com/104793540/212891790-bdfdb930-4ca3-4516-81e6-432f6fa3d2b5.png)



blocker: Docker Desktop WSL 2 backend on Windows

A significant difference when using WSL is that you will need to create and run your project files from within the Linux filesystem, not the Windows filesystem. **Going forward, all Docker commands should be run within WSL**

![image](https://user-images.githubusercontent.com/104793540/212734223-4963a1a9-c8e8-464a-b2a9-d6b85859b8d3.png)
![image](https://user-images.githubusercontent.com/104793540/212734423-56d9d539-427b-4196-8e9b-72f8a449e4f3.png)
![image](https://user-images.githubusercontent.com/104793540/212734832-3f475fc6-7594-43fb-b748-2e741a052936.png)


HOW:

-  access your Linux system by running wsl in the Windows Search Bar then `cd ~` to change into your Ubuntu user's home directory on the Linux filesystem.
- https://docs.docker.com/desktop/windows/wsl/#best-practices
- `wsl` `wsl.exe --update` `wsl -l -v` `wsl.exe --set-default-version 2` `wsl --set-default ubuntu-18.04` `wsl -l -v` `wsl`
- https://learn.microsoft.com/en-us/windows/wsl/install
- WSL extension lets you use VS Code on Windows to build Linux applications that run on the Windows Subsystem for Linux (WSL). You get all the productivity of Windows while developing with Linux-based tools, runtimes, and utilities.
- https://learn.microsoft.com/en-gb/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package
- downloaded ubuntu from store 
- https://learn.microsoft.com/en-us/windows/wsl/setup/environment#set-up-your-linux-username-and-password
- https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl
- https://www.c-sharpcorner.com/article/how-to-install-visual-studio-code-on-wsl/

![image](https://user-images.githubusercontent.com/104793540/214383801-457d8a0e-2bba-4c98-8b3d-55eef27dcb64.png)
![image](https://user-images.githubusercontent.com/104793540/214383851-dd21d06d-066d-4edd-8111-6285821c0b68.png)


## Test
Run tests associated with the project

### Step 1 Executing Tests 
Build container to run tests in:
- `docker build -f Dockerfile.dev -t ayan:frontend .`
- `docker run -it imageid npm run test`

![image](https://user-images.githubusercontent.com/104793540/212972681-14a37d26-747a-4214-b348-681dfa3c7fc6.png)


### Step 2 Docker Compose for running Tests and live updating 

````
version: "3"
services:
# first container for hosting development server 
  web:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /home/node/app/node_modules
      - .:/home/node/app
  # second container soley for running tests 
  tests:
    stdin_open: true
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /home/node/app/node_modules
      - .:/home/node/app
    command: ["npm", "run", "test"]

````

- `docker-compose up --build`

## Prod

![image](https://user-images.githubusercontent.com/104793540/214830634-9c31844f-2c9f-4709-91a6-b192237adeeb.png)
![image](https://user-images.githubusercontent.com/104793540/214830863-318468c1-a934-4463-83d7-f9efba30b2c5.png)

- creating separate Dockerfile for creating production version of web container:
- will start up an Nginx system > serves up index.html & main.js 

### Multi-Step Docker Builds 

![image](https://user-images.githubusercontent.com/104793540/215174347-da8416e6-254f-4ca6-963a-1c6c6ff99e64.png)


````
FROM node:16-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build



FROM nginx 
COPY --from=builder /app/build /usr/share/nginx/html
# check dockerhub for nginx to find usr....

````

- `docker build .`
- `docker run -p 8080:80 idimage

## CI and Deployment 

![image](https://user-images.githubusercontent.com/104793540/215553416-e8b6c2a6-c759-48a3-b30a-9ecd01116785.png)


- set up travis and github

![image](https://user-images.githubusercontent.com/104793540/215284174-cd9dc7b9-4a16-4b00-bd06-81e77432772c.png)

Travis YMl File coniguration 

```
Tell Travis i need a copy of docker running 

Build my image using Dockerfile.dev

Tell Travis how to run test suite 

Tell Travis how to deploy my code to AWS
```

- .travis.yml

```
language: generic 
sudo: requried 
services:
  - docker
  
before_install:
  - docker build -t asalad42/docker-react -f Dockerfile.dev .
  
script:
  - docker run -e CI=true asalad42/docker-react npm run test
```

### Automatic Build Creation 

- git commit and then push triggers travis job:


![image](https://user-images.githubusercontent.com/104793540/215499640-1bd85bfc-ccfb-4c67-9b25-0f61758a3c6c.png)
![image](https://user-images.githubusercontent.com/104793540/215499758-d75f12c4-7ea2-425f-a4d1-9d2640655f17.png)

- https://create-react-app.dev/docs/running-tests/#continuous-integration
- https://docs.docker.com/engine/reference/run/#env-environment-variables


### AWS Elastic Beanstalk 

- AWS Elastic Beanstalk is an easy-to-use AWS service for **deploying and scaling web applications and services**
- just upload code (i am using travis ci) and EB automatically handles the deployment, capacity provisioning, load balancing, auto-scaling and application health monitoring. 
- fastest and simplest way to deploy application to AWS while **retaining full control over the AWS resources powering application and access  to the underlying resources at any time**

![image](https://user-images.githubusercontent.com/104793540/216078996-1dd776a5-ef22-4b00-9366-a9eb25afad37.png)
![image](https://user-images.githubusercontent.com/104793540/215561780-69b74f57-f51c-4555-b3fe-731e8a85d8de.png)
![image](https://user-images.githubusercontent.com/104793540/216079134-c5e6f99f-87b8-456c-8d23-e774bc11a24b.png)

#### Automated deployments 

- updated .travis.yml file 
- new iam user docker-react-travis-ci > fullaccess > back on travis and add keys 
- push from local host to github 

```
sudo: required
language: generic

services:
  - docker

before_install:
  - docker build -t asalad42/docker-react -f Dockerfile.dev .

script:
  - docker run -e CI=true asalad42/docker-react npm run test

deploy: 
  provider: elasticbeanstalk
  region: "eu-west-1"
  app: "docker-react"
  env: "Dockerreact-env"
  bucket_name: "elasticbeanstalk-eu-west-1-670135081089"
  bucket_path: "docker-react"
  on:
    branch: main 
  access_key_id: $AWS_ACCESS_KEY
  secret_access_key: "$AWS_SECRET_KEY"

```
![image](https://user-images.githubusercontent.com/104793540/215519808-b856004d-f862-48c0-887f-698429ebc4f7.png)
![image](https://user-images.githubusercontent.com/104793540/215518313-8d7ad55a-4ec6-4011-a888-08420767902d.png)
![image](https://user-images.githubusercontent.com/104793540/215519669-91e5da36-88a3-41a8-911d-db428ceb700c.png)

- Nginx: host react source/production code files
- Dockerfile add EXPOSE 80 for nginx, makes changes in app.js file, commited changes and pushed to github
- travis ci picked up change and ran tests > deployed application to AWS.

![image](https://user-images.githubusercontent.com/104793540/215857690-5ffaa04c-9fe4-49fc-bd64-1c856a65786e.png)
![image](https://user-images.githubusercontent.com/104793540/215857872-902d8065-eb5e-4cfd-a758-a279879a3b19.png)
![image](https://user-images.githubusercontent.com/104793540/216078299-b1eff404-710b-4f21-8a00-7059bbc055bc.png)



**Redeploy on Pull Request** 

- `git checkout -b feature`
- make changes on app.js > git add, git commit
- `git push origin feature`
- compare and pull request on github > all checks have passsed > merge pull request

![image](https://user-images.githubusercontent.com/104793540/216037042-76f7e7dc-8b98-4f79-9aa2-65ce04967c28.png)
![image](https://user-images.githubusercontent.com/104793540/216037187-f3feaf7a-6c5c-495a-9bbf-638573ee185a.png)
![image](https://user-images.githubusercontent.com/104793540/216037377-5aaf642d-c12f-43fd-a2d1-796f69178076.png)

- `git branch`   `git checkout main`  `git branch -D feature`     `git push remote_name --delete branch_name`   Note: In most cases, remote_name will be origin

- Travis CI new build triggers 
- auto updated on elasticbeanstalk 

![image](https://user-images.githubusercontent.com/104793540/216084365-98453763-7213-4c3b-969a-2c3d85e25196.png)


##### Environtment cleanup

- Go to the Elastic Beanstalk dashboard.
- In the left sidebar click "Applications"
- Click the application you'd like to delete.
- Click the "Actions" button and click "Delete Application"


## Building Multi-Container Application 

Building complicated fib calculator:

![image](https://user-images.githubusercontent.com/104793540/216111376-bf817d92-cd1b-4d9e-a1e5-4fcad9950bda.png)

Development and Production Plan 

![image](https://user-images.githubusercontent.com/104793540/216108663-bbccac8b-b68d-487e-a590-ffc479b7a0df.png)
![image](https://user-images.githubusercontent.com/104793540/216108092-2f9fd928-dcc5-4b72-87ea-2c59720bcc9b.png)
![image](https://user-images.githubusercontent.com/104793540/216108188-4c88cb81-e456-4aaa-8672-fa0b333e05f0.png)

### Development 

Create react app:
- `npx create-react-app client`
- `rm -r .git`
- client > src > add new files (otherpage.js and fib.js), for routing edit App.js file

Also setup:
- worker process (index,js, keys.js, package.json > calculates fib sequence) 
- express API server (package.json with dependecies and scripts, keys.js to connect to running redis and postgres instances, index.js for logic to connect to redis and postgres) 

- `node index.js` must say cannot find module "example" otherwise there is a syntax error in index or keys files

Dev Dockerfiles:

- client: react app (2 pages user can visit in application)
- server: express server (API layer that communicates with redis and postgress)
- worker: worker (calculates value for indices and puts back into redis)

Docker compose:

![image](https://user-images.githubusercontent.com/104793540/216120527-5992c646-d023-4607-89c1-143a08e38040.png)

```
version: '3'
services:
  postgres:
    image: 'postgres:latest'
  redis:
    image: 'redis:latest'
  nginx:
    depends_on:
      - api
      - client
    restart: always 
    build: 
      dockerfile: Dockerfile.dev
      context: ./nginx
    ports:
      - '3050:80'
  api:
    depends_on:
      - redis
      - postgres 
    build: 
      dockerfile: Dockerfile.dev
      context: ./server  # . means look in current wkdir and look for dir /server
    volumes:
      - /app/node_modules # inside container dont try to override this folder
      - ./server:/app  # look inside server dir and copy everything into app folder in container 
    environment:
      - REDIS_HOST=redis  # at run time
      - REDIS_PORT=6379
      - PGUSER=postgres
      - PGHOST=postgres
      - PGDATABASE=postgres
      - PGPASSWORD=postgres_password
      - PGPORT=5432
      # all info for env variables from documentation for each service on dockerhub explorer 
  client:
    environment:
      - WDS_SOCKET_PORT=0
    build:
      dockerfile: Dockerfile.dev
      context: ./client
    volumes:
      - /app/node_modules
      - ./client:/app
  worker:
    depends_on:
      - redis
    build:
      dockerfile: Dockerfile.dev
      context: ./worker
    volumes:
      - /app/node_modules
      - ./worker:/app
    environment:
      - REDIS_HOST=redis
      - REDIS_PORT=6379

```
Routing with Nginx:

![image](https://user-images.githubusercontent.com/104793540/216769144-318f5721-f5ba-4885-8fc7-eb0214100458.png)

Aim:
- all frontend react code to one backend (requests)
- all frontend express code to one backend (api requests)

Need:

![image](https://user-images.githubusercontent.com/104793540/216769413-642eef97-554e-4f3b-bdb6-d43e8f6c21e9.png)

- default.conf file: 
```
upstream client {
    server client:3000;
}

upstream api {
    server api:5000;
}


server {
    listen 80;

    location / {
        proxy_pass http://client; 
    }


    location /ws {
        proxy_pass http://client;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
    }
    
    location /api {
        rewrite /api/(.*) /$1 break;
        proxy_pass http://api;
    }
}
```

- dockerfile for nginx

```
FROM nginx
COPY ./default.conf /etc/nginx/conf.d/default.conf
```

- add nginx to docker compose with route mapping 
- `docker-compose up --build` >> docker endpoint for "default" not found
- `docker context ls`
- fix errors then `docker-compose down`
- `docker-compose up --build`

![image](https://user-images.githubusercontent.com/104793540/216782304-37fa54ba-c530-49e8-9b22-45dba5ebc826.png)
![image](https://user-images.githubusercontent.com/104793540/216782435-6bdd676a-29eb-4f8e-b525-de5726daf6da.png)
![image](https://user-images.githubusercontent.com/104793540/216782448-e10aeb36-e2b6-4bf1-8031-a0785aed9787.png)

**Both pages and calculations working**

Troubleshooting:
- WebSocket connection failed
- opening websocket connections
- solution: set an environment variable on the client service in the docker-compose.yml


![image](https://user-images.githubusercontent.com/104793540/216785296-33862609-6496-4b96-bd43-7db8f222e0f6.png)

## Continous Integration Workflow for Multiple Images

![image](https://user-images.githubusercontent.com/104793540/216783859-56015724-5226-4564-90e4-a72aa57a4087.png)


Production Dockerfiles:
- worker
- server
- client
- nginx 

Travis Config File:

![image](https://user-images.githubusercontent.com/104793540/216785085-b66c62e2-c86a-4a7c-8f5f-12bdd8d9d0a8.png)

- link github and travis ci 
- .travis.yml 
- use env variables for docker login cred
- on travis repo for project > more options > environment variables 

![image](https://user-images.githubusercontent.com/104793540/216789867-65327612-eaa0-48e7-b1e6-7b9bea16d436.png)

connecting Travis CI to dockerhub:

```
# log in to docker cli 
  - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_ID" --password-stdin
```

partially complete travis yml:
````
sudo: required
language: generic

services:
  - docker

before_install:
  - docker build -t asalad42/react-test -f ./client/Dockerfile.dev ./client

# path to file and look into client dir to ger build context 

script:
  - docker run -e CI=true asalad42/react-test npm run test

after_success: 
  - docker build -t asalad42/multi-client ./client 
  - docker build -t asalad42/multi-nginx ./nginx
  - docker build -t asalad42/multi-server ./server  
  - docker build -t asalad42/multi-worker ./worker  

# log in to docker cli 
  - echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_ID" --password-stdin

# take these production images and push to docker hub 
  - docker push asalad42/multi-client
  - docker push asalad42/multi-nginx
  - docker push asalad42/multi-server
  - docker push asalad42/multi-worker
  
````

- push to github and wait for ci to finish, check dockerhub for pushed images

![image](https://user-images.githubusercontent.com/104793540/216817702-a9e88470-afc9-46ff-9b03-8b76dab1cf89.png)
![image](https://user-images.githubusercontent.com/104793540/216817642-f7d08adc-6049-4c96-9d07-f0e4bdfa468f.png)

- check travis logs for issues such as typo in syntax of dockerfiles and incorrect config in travis yml file. Helped me fix both issues so very useful 

## Multi-Container Deployments to AWS
![image](https://user-images.githubusercontent.com/104793540/216856230-788a5864-727b-46e5-bd0a-1433e72cefa5.png)

### EBS Application Creation

 https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#container_definitions
- Amazon ECS task definitions > Task definition parameters > Container definitions

![image](https://user-images.githubusercontent.com/104793540/216957400-efdd6cb8-c760-4c74-b309-8035a114b03a.png)

### RDS Database Creation
![image](https://user-images.githubusercontent.com/104793540/216856299-2d994f58-d41b-4d6f-a2e7-a8dd6c85de6e.png)

- RDS > PostgreSQL > template free tier
- master username and master password > env variables in docker compose file 
- create database 

![image](https://user-images.githubusercontent.com/104793540/216962530-6159b49e-80c5-4fc7-9082-02c928c1a3bc.png)

### ElastiCache Redis Creation
![image](https://user-images.githubusercontent.com/104793540/216856269-b12415fa-12b5-4e9e-b404-a2a00eb7b6bf.png)

- ElastiCache > Redis clusters > create
- Make sure Cluster Mode is DISABLED.
- Scroll down to Cluster settings and change Node type to cache.t2.micro

![image](https://user-images.githubusercontent.com/104793540/216963626-7cc0d7bf-0610-4d2c-8f97-6275363841cd.png)

### Creating a Custom Security Group
![image](https://user-images.githubusercontent.com/104793540/216957610-5d75d57e-8ca6-4363-9c64-2599e66a304b.png)

- multi-docker sg for traffic for services in multi-docker app

![image](https://user-images.githubusercontent.com/104793540/217014022-abf6274b-41c4-439f-acd3-77d7d23f52cc.png)

#### Applying Security Groups to ElastiCache
![image](https://user-images.githubusercontent.com/104793540/216969132-7cffdbb5-7ec0-4498-a7f3-261f9b86dfd6.png)

#### Applying Security Groups to RDS
![image](https://user-images.githubusercontent.com/104793540/216969799-f6784dc5-14a3-47da-9e45-fcb12af9881f.png)
![image](https://user-images.githubusercontent.com/104793540/216970281-44f4d47b-3ab9-4d9c-b62e-ea6b2c4d6e33.png)

#### Applying Security Groups to Elastic Beanstalk
![image](https://user-images.githubusercontent.com/104793540/216970650-e89ea545-fc8b-40e8-b859-d9425c7ba032.png)
![image](https://user-images.githubusercontent.com/104793540/216970704-3f11c07b-966c-425e-a38f-94b958029ca6.png)

### Add AWS configuration details to .travis.yml file's deploy script
```
deploy:
  provider: elasticbeanstalk
  region: 'eu-west-1'
  app: 'multi-docker'
  env: 'Multidocker-env'
  bucket_name: 'elasticbeanstalk-eu-west-1-670135081089'
  bucket_path: 'docker-multi'
  on:
    branch: main
  access_key_id: $AWS_ACCESS_KEY
  secret_access_key: $AWS_SECRET_KEY

````
### Setting Environment Variables
For both express API and worker to function correctly, environment variables must be provided:

![image](https://user-images.githubusercontent.com/104793540/216973944-ec510c1a-f8f3-49d4-ad59-864a94f84860.png)

### IAM Keys for Deployment
- create user with permission AdministratorAccess-AWSElasticBeanstalk
- generate access keys to put into travis ci 

### AWS Keys in Travis 
- while in multi-docker project > settings > env var

![image](https://user-images.githubusercontent.com/104793540/216976706-b4879d90-b0ee-40bc-bced-29c10195fc8f.png)

### Deploying App

- save changes on local host > commit > push 
- github picks up changes > travis ci picks up code 

![image](https://user-images.githubusercontent.com/104793540/216983350-a318b1e6-8c67-4167-80f3-27692f25c5f6.png)
![image](https://user-images.githubusercontent.com/104793540/216983407-5fb3c909-4f57-45ca-be7e-271d0f798c45.png)
![image](https://user-images.githubusercontent.com/104793540/216983466-4b4b409b-1431-4ce8-b6dd-f8a8312e326d.png)
![image](https://user-images.githubusercontent.com/104793540/216983673-a4548b75-56ec-4aec-ac67-625ae8eb8ca7.png)

checking calculations and if databases hold values:

![image](https://user-images.githubusercontent.com/104793540/217014630-1ebabbfe-71a7-41f4-934d-20dffbd24660.png)


##### Cleaning up AWS Resources 
- EB, RDS, EC

# Deployment of JavaScript-NodeJs application using Docker,Nexus and DigitalOcean

<p><img src="https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)">   
<img src="https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white">
<img src="https://img.shields.io/badge/DigitalOcean-%230167ff.svg?style=for-the-badge&logo=digitalOcean&logoColor=white">
<img src="https://img.shields.io/badge/MongoDB-%234ea94b.svg?style=for-the-badge&logo=mongodb&logoColor=white">
<img src="https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white">
</p>

## Technologies Used

- <h5>NodeJs , Docker , AWS ECR , Nexus , MongoDB , Mongo-Express , DigitalOcean<h5>
<hr>

## Project Description
- Git cloned an existing project consisting of ***Javascript-Nodejs*** application. Pulled ***Mongodb*** and ***Mongo-express*** from ***DockerHub*** and  
  created mongo-network to start containers in it.Started Mongodb and Mongoexpress container with all the necessary parameters and  
  created new database via Mongo Express Ui.Configured Nodejs application code to connect with database.
- Created a docker-compose file to start mongodb and mongo-express containers instead of using docker run.Created a Dockerfile for our 
  Nodejs Application and built a Docker image.Created private Docker Registry on ***AWS ECR*** and pushed the Docker image to AWS ECR 
  repository.
- Added the application image in Dockerfile services and started the Docker Container with Docker-compose.Configured persistence for  
  our application data using Docker volumes.
- Created a Docker repository on ***Nexus*** and Configured User Role,Ports and Firewall rules.Pushed the Docker Image to Nexus Repository.
  Created a Droplet on ***DigitalOcean*** Server and installed Docker.Ran Nexus as a Docker Container on DigitalOcean Server and accessed it 
  from browser.

<hr>

## Setup

### With Docker
__Step 1:__  Create docker network
```
docker network create mongo-network 
```
__Step 2:__ Start Mongodb
```
docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo-network \
mongo:5.0
```
__Step 3:__ Start Mongo-express
```
docker run -d \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_MONGODB_ENABLE_ADMIN=true \
--net mongo-network \
--name mongo-express \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
mongo-express:0.54.0
```
NOTE: Creating docker-network is optional. You can start both containers in a default network. In this case, just emit ```--net``` flag in ```docker run``` command

__Step 4:__ Open Mongo-express from browser
```
http://localhost:8081
```
__Step 5:__ Create ```user-account``` db and ```users``` collection in mongo-express

__Step 6:__ Start your nodejs application locally - go to ```app``` directory of project
```
cd app
npm install 
node server.js
```
___Step 7:___ Access you nodejs application UI from browser
```
http://localhost:3000
```

### With Docker Compose
___Step 1:___ Start mongodb and mongo-express
```
docker-compose -f docker-compose.yaml up

```
You can access the mongo-express under localhost:8081 from your browser

___Step 2:___ In mongo-express UI - create a new database "user-account"

___Step 3:___ In mongo-express UI - create a new collection "users" in the database "user-account"

___Step 4:___ Start node server
```
cd app
npm install
node server.js
```
___Step 5:___ Access the nodejs application from browser
```
http://localhost:3000

```

### To build a docker image from the application
```
docker build -t my-app:1.0 .  
```
The dot "." at the end of the command denotes location of the Dockerfile

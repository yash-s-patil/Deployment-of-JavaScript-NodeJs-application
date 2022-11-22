# commands

## download docker images of mongodb and mongoexpress
docker pull mongo:5.0
docker pull mongo-express:0.54.0

## create docker network 
docker network create mongo-network

## start mongodb 
docker run -d \
-p 27017:27017 \
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--name mongodb \
--net mongo-network \
mongo:5.0

## start mongo-express
docker run -d \
-p 8081:8081 \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
-e ME_CONFIG_MONGODB_ENABLE_ADMIN=true \
--net mongo-network \
--name mongo-express \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
mongo-express:0.54.0

## check wheather databse has started and mongo and mongo-express are connected
docker logs CONTAINER_ID(of mongo-express)


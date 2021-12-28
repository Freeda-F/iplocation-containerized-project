# IP Location containerized Project

This is a three tier Flask application which consists of the following :
  - Frontend / GUI
  - API service
  - Backend

In this module, we will show how each of these layers can be completely dockerised and has the ability to easily make images off of the application for pushing into production.

## Features

This application will provision 3 layers :

  - Frontend / GUI - It is an application code in Flask which displays the information of the given IP address (like Continent Name, Continent Code, Country Name, ISP and cache information) in a user-friendly interface. 
  
  - API service - It is an application code in Flask which display all the IP information in JSON format. 
  
  - Backend - It is a REDIS database for caching purpose which stores the IP and its location based on key-value structure.

## Requirements
- [Install docker](https://docs.docker.com/engine/install/)
- [Install docker-compose](https://docs.docker.com/compose/install/)
- An [API KEY](https://app.ipgeolocation.io/) for finding the geolocation.
- The above API_KEY has to be stored in [AWS secret Manager](https://aws.amazon.com/secrets-manager/) for accessing it securely.
- The Instances to deplay docker container should have an IAM ROle - ['SecretsManagerReadWrite'](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_available-policies.html) attached to itself.

## Building Docker images
For deploying the 3 layer flask application, we need to build custom images for Frontend and API service using a Dockerfile.

#### Frontend / GUI

The files required for creating the image for Frontend/ GUI is in the folder - "iplocation-gui". You may download the contents of this folder and execute the below commands to create a docker image or you can simply pull the premade image in step 5.

1. Run the Dockerfile
```
docker build -t freedafrancis/iplocation-gui:v1 .
```
3. You can give the required tags to the docker image
```
docker tag freedafrancis/iplocation-gui:v1 freedafrancis/iplocation-gui:latest
```
4. Push the image to the repository
```
docker image push freedafrancis/iplocation-gui:v1
docker image push freedafrancis/iplocation-gui:latest
```
5. Download precreated image
```
docker image pull freedafrancis/iplocation-gui:latest
```

#### API Service

The files required for creating the image for API Service is in the folder - "iplocation-api". You may download the contents of this folder and execute the below commands to create a docker image or you can simply pull the premade image in step 5.

1. Run the Dockerfile
```
docker build -t freedafrancis/iplocation-api:v1 .
```
3. You can give the required tags to the docker image
```
docker tag freedafrancis/iplocation-api:v1 freedafrancis/iplocation-api:latest
```
4. Push the image to the repository
```
docker image push freedafrancis/iplocation-api:v1
docker image push freedafrancis/iplocation-api:latest
```
5. Download precreated image
```
docker image pull freedafrancis/iplocation-api:latest
```

## Provisioning

1. The API_KEY in the application is stored as a secret in AWS secret manager.
![image](https://user-images.githubusercontent.com/93197553/147587836-ed0dba63-93fe-4dbb-8847-a869ccee7a68.png)


## Run the containers 

In this demo, the containers are run manually using docker commands. You may also use [docker-compose](https://docs.docker.com/compose/install/) commands to automatically deploy and run the containers. You may refer [this demo]() for the same. 

1. REDIS Container
```
docker container run \
-d \
--name redis \
--network iplocation-net \
--restart always \
redis:latest
```

2. IP Location - API Service container
```
docker container run \
-d \
-p 8080:8080 \
--name iplocation-api \
--restart always \
--network iplocation-net \
-e REDIS_HOST="redis" \
-e APP_PORT="8080" \
-e API_KEY_FROM_SECRETSMANAGER=True \
-e SECRET_NAME="ipstack" \
-e SECRET_KEY="ipstack-api-key" \
-e REGION_NAME="ap-south-1" \
freedafrancis/iplocation-api:v1
```
3. IP Location - GUI Container
```
docker container run \
-d \
-p 80:8080 \
--restart always \
--network iplocation-net \
--name ipgeolocation-gui \
-e API_SERVER="iplocation-api" \
-e API_SERVER_PORT="8080" \
-e API_PATH="/api/v1/" \
-e APP_PORT="8080" \
freedafrancis/iplocation-gui:v1
```

![image](https://user-images.githubusercontent.com/93197553/147588332-8d7611b5-5d28-4d2f-9a30-2fd4c6540189.png)

## Result

You can access the Main application in the format - http://domain.com/ip/123.5.6.6
![image](https://user-images.githubusercontent.com/93197553/147588525-34b7fa80-0968-4984-bc1f-8f8598a7642b.png)

You may also access the RAW JSON contents using the format - http://domain.com:8080/api/v1/123.5.6.6
![image](https://user-images.githubusercontent.com/93197553/147588754-544497d1-a752-43a3-a15c-36c3fc58cd4f.png)


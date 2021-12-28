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

You can either deploy the containers manually using docker commands or using [docker-compose](https://docs.docker.com/compose/install/). I will be deploying this application using docker-compose.

#### Docker-compose
Create a docker-compose file(docker-compose.yml) and use compose commands to deploy the container.

1. Check if the docker-compose.yml file has a valid format
```
docker-compose config
```
![image](https://user-images.githubusercontent.com/93197553/147594193-db14679e-425e-4cb3-9844-cd1b4a597ffc.png)

2. Use docker compose command to deploy all the containers
```
docker-compose up -d
```
![image](https://user-images.githubusercontent.com/93197553/147594293-38f20841-2edf-4f78-9087-a4810084cf6b.png)

## Result

You can access the Main application GUI in the format - http://domain.com/ip/123.5.6.6
![image](https://user-images.githubusercontent.com/93197553/147588525-34b7fa80-0968-4984-bc1f-8f8598a7642b.png)

You may also access the RAW JSON contents using the format - http://domain.com:8080/api/v1/123.5.6.6
![image](https://user-images.githubusercontent.com/93197553/147588754-544497d1-a752-43a3-a15c-36c3fc58cd4f.png)


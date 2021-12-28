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
- An [API KEY](https://app.ipgeolocation.io/) for finding the geolocation

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

## Run the container


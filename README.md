# Plora Tran Udagram Refactor Monolith -> Microservices Submission

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into three parts:
1. [The Simple Frontend](./udacity-c3-frontend)
A basic Ionic client web application which consumes the RestAPI Backend. 
- **Frontend:** port 8100. 
- port-forward 8100:8100
- frontend use port 80 but forward to port 8100 of the localhost. 
- **Command:** `npm run start` or `ionic serve`
2. [The RestAPI Feed Backend](./udacity-c3-restapi-feed), a Node-Express feed microservice.
- **Feed Backend:** port 8080
- **Command:** `npm run dev` or `npm run prod` 
3. [The RestAPI User Backend](./udacity-c3-restapi-user), a Node-Express user microservice.
- **User Backend:** port 8080. 
- don't need to portforward port 8080 to localhost due to passs reverseproxy port
- **Command:** `npm run dev` or `npm run prod` 
4. **Revers Proxy** port 8080
- port-forward 8080:8080
- Reverseproxy runs at port 8080, same as backend-feed and backend-user server. 

## Steps to do in the project: 

1. Login to [Docker Hub](https://hub.docker.com/) and create 4 repositories, corresponding to: 
- restapi-feed
- restapi-user
- frontend
- reverseproxy

2. Use [Travis](https://travis-ci.org/) for Continuous Integration to build Docker containers. Then verify with Docker hub to check if docker containers are successfully built. 

4. Apply secrets to K8s cluster: 
`kubectl apply -f aws-secret.yaml env-configmap.yaml env-secret.yaml`

5. Apply services to K8s cluster: 
`kubectl apply -f backend-feed-service.yaml backend-user-service.yaml frontend-service.yaml reverseproxy-service.yaml`

6. Deploy Pods to K8s cluster using Docker containers:
`kubectl apply -f backend-feed-deployment.yaml backend-user-deployment.yaml frontend-deployment.yaml reverseproxy-deployment.yaml`

7. Verify if everything is deployed to K8s cluster: 
`kubectl get pods`
`kubectl get services`
`kubectl get configmaps`
`kubectl get secrets`

8. Since inside K8s cluster, we have multiple nodes running. Thus, load balancing to the port is done by the services. Port-forward the services to verify the app is running. Open 2 terminals and run: 
`kubectl port-forward service/reverseproxy 8080:8080`
`kubectl port-forward service/frontend 8100:8100`

9. Open browser, type: `localhost:8100` to use the app. 

## Getting Setup

> _tip_: this frontend is designed to work with the RestAPI backends). It is recommended you stand up the backend first, test using Postman, and then the frontend should integrate.

### Installing Node and NPM
This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (NPM is included) from [https://nodejs.com/en/download](https://node js.org/en/download/).

### Installing Ionic Cli
The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI can be found in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

### Installing project dependencies

This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the root of this repository. After cloning, open your terminal and run:
```bash
npm install
```
>_tip_: **npm i** is shorthand for **npm install**

### Setup Backend Node Environment
You'll need to create a new node server. Open a new terminal within the project directory and run:
1. Initialize a new project: `npm init`
2. Install express: `npm i express --save`
3. Install typescript dependencies: `npm i ts-node-dev tslint typescript  @types/bluebird @types/express @types/node --save-dev`
4. Look at the `package.json` file from the RestAPI repo and copy the `scripts` block into the auto-generated `package.json` in this project. This will allow you to use shorthand commands like `npm run dev`


### Configure The Backend Endpoint
Ionic uses enviornment files located in `./src/environments/environment.*.ts` to load configuration variables at runtime. By default `environment.ts` is used for development and `environnment.prod.ts` is used for produciton. The `apiHost` variable should be set to your server url either locally or in the cloud.

***
### Running the Development Server
Ionic CLI provides an easy to use development server to run and autoreload the frontend. This allows you to make quick changes and see them in real time in your browser. To run the development server, open terminal and run:

```bash
ionic serve
```

### Building the Static Frontend Files
Ionic CLI can build the frontend into static HTML/CSS/JavaScript files. These files can be uploaded to a host to be consumed by users on the web. Build artifacts are located in `./www`. To build from source, open terminal and run:
```bash
ionic build
```
***
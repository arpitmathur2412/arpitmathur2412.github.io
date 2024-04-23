---
title: "Deploying a Full Stack Web Application using Docker"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Post Formats
  - readability
  - standard
---

Hey everyone, In this blog post we will be deploying a full stack web application using Docker. The application will be split into three microservices. 

1. Frontend Container of ReactJs 
2. Backend Container of NodeJs
3. Database Container of MongoDB

Before we start with the deployment, let's understand what Docker is and why we need it.
Docker is a platform that allows you to develop, test, and deploy applications in containers. Containers are lightweight, standalone, and executable packages of software that include everything needed to run an application: code, runtime, system tools, system libraries, and settings. Containers are isolated from each other and from the underlying host, which means that they can run the same application on different environments without any changes. This makes it easy to deploy applications in a consistent and reproducible way. 

Docker also provides tools for managing containers, such as Docker Compose, which allows you to define and run multi-container applications. In this blog, we will be using Docker Compose to define and run our full stack web application. Let's start

#### 1. Create a full stack application or use an existing one built on ReactJs, NodeJs and MongoDB.


I have used an application of my own which you can clone from [Stock Suggestion App](https://github.com/arpitmathur2412/GFG-HACK) or use any existing application you have. Just make sure it has the same tech-stack.

This is the file structure of our application:

```bash
GFG-HACK/
└── client/
└── server/
```

#### 2. Create a repository on Docker Hub to store your Docker images.

Go to Docker Hub and create a new repository to store your Docker images. You can create a public or private repository depending on your requirements. Once you have created the repository, note down the repository name as we will need it later to push our Docker images to the repository.

This is what your repository should look like  
<br>
![Docker Hub](/assets/images/image.png)  
<br>


#### 3. Create a Dockerfile for each of the services.

Dockerfile is a text file that contains a set of instructions that are used to build a Docker image. The Docker image is a lightweight, standalone, and executable package of software that includes everything needed to run an application. The Dockerfile contains instructions to build the image, such as the base image, working directory, dependencies, and commands to run the application.  
<br>


**3.1 Dockerfile for ReactJs server at path client/Dockerfile**

Before creating the Dockerfile for the ReactJs server, make sure you have a build script in your package.json file. If not, add the following script to your package.json file:

```json
"scripts": {
"start": "react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test",
"eject": "react-scripts eject"
}
```  
<br>

1. Now, change your current working directory to client  
<br>
```bash 
  cd client
``` 

2. create a Dockerfile in the client folder  
<br>
```bash
  touch Dockerfile
```

**Open the Dockerfile in a text editor of your choice and follow the below steps:**

- Use the official Node.js image as the base image.  
<br>
```Dockerfile
FROM node:16
```

- Set the working directory inside the container.  
<br>
````Dockerfile
WORKDIR /app
````

- Copy package.json and package-lock.json to the working directory.  
<br>
````Dockerfile
COPY package*.json ./
````

- Install the project dependencies.  
<br>
````Dockerfile
RUN npm install
````

- Copy the rest of the frontend code to the working directory.  
<br>
````Dockerfile
COPY . .
````

- Build the React app for production.  
<br>
````Dockerfile
RUN npm run build
````

- Expose the port on which your frontend app will run.  
<br>
````Dockerfile
EXPOSE 3000
````

- Serve the built React app using a simple web server.  
<br>
```Dockerfile
CMD ["npx", "serve", "-s", "build"]
````  
<br>
**This is what your final Dockerfile should look like:**

```Dockerfile
FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npx", "serve", "-s", "build"]
```  
<br>


**Now we will build the Docker image from the created Dockerfile and push it to the Docker Hub repository, follow the below steps carefully**

1. Build the Docker image using the following command:
  
  ```bash 
  docker build -t <DockerHubUsername>/<RepositoryName>:client .
  ```  
  <br>
  ![docker build for client](/assets/images/client-build.png)  
  <br>

  Replace ```<DockerHubUsername>``` with your Docker Hub username and ```<RepositoryName>``` with the repository name of the created repository in step 2.   
  <br>
  This command will build the Docker image for the ReactJs server using the Dockerfile in the client folder.

2. Lets check if the image is created successfully by running the following command:
  
  ```bash 
  docker images
  ```  
  <br>
  ![docker images](/assets/images/docker_images.png)  
  <br>
  This command will list all the Docker images on your system. You should see the Docker image for the ReactJs server in the list.

3. Now we will see if the image is running successfully by running the following command:
  
  ```bash 
  docker run -p 3000:3000 <DockerHubUsername>/<RepositoryName>:client
  ```  
  <br>

  ![docker run](/assets/images/frontend-run.png)    
  <br>
  ![frontend-browser](/assets/images/frontend-browser.png)  
  
  <br>
  This command will run the Docker image for the ReactJs server on port 3000. You should be able to access the ReactJs server at http://localhost:3000.


4. Push the Docker image to the Docker Hub repository using the following command:
  
  ```bash 
  docker push <DockerHubUsername>/<RepositoryName>:client
  ```  
  <br>

  ![docker frontend](/assets/images/frontend_push.png)  
  <br>

  This command will push the Docker image to the Docker Hub repository. You can check the Docker Hub repository to see if the image has been pushed successfully.  
  <br>

  ![frontend_push_check](/assets/images/frontend_push_check.png)



**3.2 Creating Dockerfile for NodeJs server**  
<br>

1. Now, change your current working directory to client  
<br>
```bash 
  cd server
``` 

2. Create a Dockerfile in the client folder  
<br>
```bash
  touch Dockerfile
```

**Open the Dockerfile in a text editor of your choice and follow the below steps:**

- Use the official Node.js image as the base image.  
<br>
```Dockerfile
FROM node:16
```

- Set the working directory inside the container.  
<br>
````Dockerfile
WORKDIR /app
````

- Copy package.json and package-lock.json to the working directory.  
<br>
````Dockerfile
COPY package*.json ./
````

- Install the project dependencies.  
<br>
````Dockerfile
RUN npm install
````

- Copy the rest of the frontend code to the working directory.  
<br>
````Dockerfile
COPY . .
````

- Expose the port on which your frontend app will run.  
<br>
````Dockerfile
EXPOSE 5000
````

- Serve the built React app using a simple web server.  
<br>
```Dockerfile
CMD ["npm", "start"]
````  
<br>
**This is what your final Dockerfile should look like:**

```Dockerfile
FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5000

CMD ["npm", "start"]
```  
<br>


**Now we will build the Docker image from the created Dockerfile for the server and push it to the Docker Hub repository, follow the below steps carefully**

- Build the Docker image using the following command:
  
```bash 
docker build -t <DockerHubUsername>/<RepositoryName>:server .
```  
<br>
![docker build server](/assets/images/docker_build_server.png)    
<br>

Replace ```<DockerHubUsername>``` with your Docker Hub username and ```<RepositoryName>``` with the repository name of the created repository in step 2.   
<br>
This command will build the Docker image for the Nodejs server using the Dockerfile in your local repository.


**Note**: We will not run the server image as it requires a connection to the MongoDB database. We will run the server image after creating the MongoDB image.

- Push the Docker image to the Docker Hub repository using the following command:
  
```bash 
  docker push <DockerHubUsername>/<RepositoryName>:server
```  
<br>

![docker server push](/assets/images/docker_server_push_.png)  
<br>

This command will push the Docker image to the Docker Hub repository. You can check the Docker Hub repository to see if the image has been pushed successfully.  
<br>  

![server_push_check](/assets/images/server_push_check.png)  
<br>


Now we have our client and server images pushed to the Docker Hub repository and ready to run. We now will pull an already created MongoDB image from the Docker Hub and run it.

- **3.3 Pulling MongoDB image from Docker Hub**

- Pull the MongoDB image from the Docker Hub using the following command:
  
```bash
docker pull mongo:latest
```
![mongo pull](/assets/images/mongo-pull.png)  
<br>


### 4. Run our application with the created docker images

Now that we have all our images ready, we can run our application by running all the docker containers and exposing the ports on which they will run so all three microservices can communicate with each other.
    
Follow the below commands to run your containers:  
<br>

```bash
docker run -dp 3000:3000 <DockerHubUsername>/<RepositoryName>:client
docker run -dp 27017:27017 mongo
```
<br>
Note: we have used -d tag so that the containers can run in the background without blocking the terminal 

Now that we have created the containers, we need to create a network that we will connect to the server and the mongo container. We will do this so that we can get the IP of the mongo container and use it in the connection string as ````mongoose.connect('mongodb:<CONTAINER_IP>:27017:<DB_NAME>') ```` 

This is how your create a docker network:  
<br>

![create network](/assets/images/create_net.png)  
<br>

Now you attach this network to the mongodb container:
```` docker network connect <NETWORK_NAME> <CONTAINER_ID>````  
<br>   

![mongo-connect-net](/assets/images/mongo-net-connect.png)
<br>

You can get the container IP using  ```` docker inspect <CONTAINER_NAME>````  
<br>

![network inspect](/assets/images/inspect.png)  
<br>

After this, use this container IP or container ID in the the mongoose connection string and build your server image and run it again. Attach the network to the server container too to allow communication between server and database. Note: You do not have to connect the network to the client container

![server-net](/assets/images/server-net.png)  

<br>
![run_container](/assets/images/run_container.png)  
<br>

**Once all the containers are up and running, you can test the app at** ```` http://localhost:3000 ````

This is how the website will look like:  
<br>

![home](/assets/images/frontend-browser.png)  
<br>  
![about_us](/assets/images/about.png)  
<br>
![final](/assets/images/stock.png)  
<br>


**We have successfully deployed our three-tier MERN stack application with docker!**

#### Conclusion:
By containerizing your MERN stack application with Docker and leveraging Docker networks, you've achieved a portable, efficient, and scalable development environment. This approach streamlines development workflows, simplifies collaboration, and paves the way for seamless deployment to production.

Remember, this is just the beginning!  Explore further optimizations for production environments, such as volume management for persistent data storage and integrating with container orchestration tools like Kubernetes. As your MERN application grows, Docker will continue to be a valuable asset in your development toolbox.






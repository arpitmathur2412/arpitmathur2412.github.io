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



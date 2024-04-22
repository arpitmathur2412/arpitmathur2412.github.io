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



#### 1. Create a full stack application or use an existing one built on ReactJs, NodeJs and MongoDB.


I have used an application of my own which you can clone from: [Stock Suggestion App](https://github.com/arpitmathur2412/GFG-HACK) or use any existing application you have. Just make sure it has the same tech-stack.

This is the file structure of our application:

```bash
GFG-HACK/
└── client/
└── server/
```


#### 2. Create a Dockerfile for each of the services.



**2.1 Dockerfile for ReactJs server at path client/Dockerfile**
  

Before creating the Dockerfile for the ReactJs server, make sure you have a build script in your package.json file. If not, add the following script to your package.json file:

```json
"scripts": {
"start": "react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test",
"eject": "react-scripts eject"
}
```


1. Now, change your current working directory to client

  ```bash 
  cd client
  ``` 

2. create a Dockerfile in the client folder

  ```bash
  touch Dockerfile
  ```

3. Open the Dockerfile in a text editor of your choice and follow the below steps:


  1. Use the official Node.js image as the base image.

      ````Dockerfile
      FROM node:16
      ````

  2. Set the working directory inside the container.

    ````Dockerfile
    WORKDIR /app
    ````

  3. Copy package.json and package-lock.json to the working directory.

    ````Dockerfile
    COPY package*.json ./
    ````

  4. Install the project dependencies.

    ````Dockerfile
    RUN npm install
    ````

  5. Copy the rest of the frontend code to the working directory.

    ````Dockerfile
    COPY . .
    ````

  6. Build the React app for production.

    ````Dockerfile
    RUN npm run build
    ````

  7. Expose the port on which your frontend app will run.

    ````Dockerfile
    EXPOSE 3000
    ````

  8. Serve the built React app using a simple web server. 

    ```Dockerfile
    CMD ["npx", "serve", "-s", "build"]
    ````


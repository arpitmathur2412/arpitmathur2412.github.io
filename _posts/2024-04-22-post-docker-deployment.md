---
title: "Deploying a three-tier web application with Docker"
date: 2024-04-22T20:38:30-04:00
categories:
  - blog
tags:
  - Docker
  - Deployment
---

Hey everyone, In this blog post we will be deploying a full stack web application using Docker. The application will be split into three microservices. 

1. Frontend Container of ReactJs 
2. Backend Container of NodeJs
3. Database Container of MongoDB


### 1. Create a full stack application or use an existing one built on ReactJs, NodeJs and MongoDB.

I have used an application of my own which you can clone from: [Stock Suggestion App](https://github.com/arpitmathur2412/GFG-HACK) or use any existing application you have. Just make sure it has the same tech-stack.

This is the file structure of the application:

```bash
GFG-HACK/
└── client/
└── server/
```

The client folder contains the ReactJs frontend code and the server folder contains the NodeJs backend code.


### 2. Create a Dockerfile for each of the services.


#### 2.1 Dockerfile for ReactJs server at path client/Dockerfile
  
Before creating the Dockerfile for the ReactJs server, make sure you have a build script in your package.json file. If not, add the following script to your package.json file:

```json
"scripts": {
"start": "react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test",
"eject": "react-scripts eject"
}
```

Next, create a ```Dockerfile``` in the client folder with the following content

  ``Dockerfile
  # Use an official Node.js runtime as the base image
  FROM node:16

  # Set the working directory inside the container
  WORKDIR /app

  # Copy package.json and package-lock.json to the working directory
  COPY package*.json ./

  # Install the project dependencies
  RUN npm install

  # Copy the rest of the frontend code to the working directory
  COPY . .

  # Build the React app for production
  RUN npm run build

  # Expose the port on which your frontend app will run
  EXPOSE 3000

  # Serve the built React app using a simple web server
  CMD ["npx", "serve", "-s", "build"]

  ```

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```
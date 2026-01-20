# Lesson: Cloud Native Application - Local Containerization

## Lesson Objectives

By the end of this lesson, learners will be able to:

1. **Explain** the concept of containerization and its benefits in modern software delivery.
2. **Differentiate** between containers and virtual machines in terms of architecture and use cases.
3. **Build and run** a Docker image for a Spring Boot application on a local machine.
4. **Apply** multi-stage Docker builds to create optimized production-ready images.


## Lesson Overview

This lesson starts the learners on the subject of containerization, which would be used throughout the entire DevOps module. The actual implementation of containerization in real world scenarios can be complex. In this lesson, we aim to get learners up to speed by understanding simple concepts about containerization and being able to launch it locally in their laptop.

---

### Preparation

For this DevOps module, we will use a **new Spring Boot DevOps demo project** that you create and keep on your **local machine**, and we will reuse the **same project** across upcoming DevOps lessons (Docker, Docker Compose, CI/CD, monitoring, DevSecOps).

---

## Part 1 - What is Containerization

Containerization is a software deployment process that bundles an application's code with all the files and libraries it needs to run on any infrastructure. 

Traditionally, to run any application on your computer, you had to install the version that matched your machine’s operating system.

### What are Containers?

Containers are lightweight packages of your application code together with dependencies such as specific versions of programming language runtimes and libraries required to run your software services.

<img src="https://middleware.io/wp-content/uploads/2021/12/containerization-explained.jpg" width="50%">

### Containers vs Virtual Machines

Containers and virtual machines are deployment technologies that make applications independent from infrastructure resources.

Containers essentially are packaged application images that contain all the necessary files and dependencies to run the application.

Virtual machines, on the other hand, involves creating an guest operating system as well as virtualizing the underlying physical infrastructure.

<img src="https://s7280.pcdn.co/wp-content/uploads/2018/08/containers-vs-virtual-machines-810x413.png">

### Benefits of Containerization

Containerization can provide these benefits for applications:
- Portability - Allows "write once, run anywhere" by packaging the application with its dependencies.
- Efficiency - Improves efficiency by using all available resources and minimizing overhead.
- Agility - Allows for rapid deployment to any environment. 
- Faster delivery - Containerization can divide a large application into discrete parts using microservices.
- Improved security - Improves security by isolation, wherein even if a container is compromised, other containers on the same host remain secure.
- Faster app startup - Containers are lightweight so startup times are virtually instantaneous.
- Easier management - Container images can easily be managed using Docker Desktop and other tools such as Kubernetes
- Flexibility - Microservices can become flexible such that certain elements can be hosted on bare metal or deployed to virtual cloud environments.

### Containerization Use Cases

Containerization can be used for different use cases, here are some of them: 

1. Migration from Monolithic applications to Microservices
2. Setting up and standardization of development environments
3. Infrastructure as Code
4. Hybrid cloud setups
5. Disaster Recovery

---

## Part 2 - Docker

For containerization, we will be using Docker.

### What is Docker?
Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run.

### When to use Docker?

Docker can be used for the following:

- MICROSERVICES - Build and scale distributed application architectures by taking advantage of standardized code deployments using Docker containers.
- CONTINUOUS INTEGRATION & DELIVERY - Accelerate application delivery by standardizing environments and removing conflicts between language stacks and versions.
- DATA PROCESSING - Provide big data processing as a service. Package data and analytics packages into portable containers that can be executed by non-technical users.
- CONTAINERS AS A SERVICE - Build and ship distributed applications with content and infrastructure that is IT-managed and secured.

### Writing Dockerfiles

Docker can build images automatically by reading the instructions from a `Dockerfile`. A `Dockerfile` is a text document that contains all the commands a user could call on the command line to create an image.

The format of a dockerfile is as follows:

```dockerfile
#comment
INSTRUCTION arguments
```

*Note: The instruction is not case-sensitive. However, convention is to make them UPPERCASE to distinguish them from arguments.*

---

### DevOps demo project (Spring Boot)

For this DevOps module, you will use **one Spring Boot DevOps demo project on your local machine** and reuse it for every DevOps lesson.

**Where to save it (important):** Create and keep your project in a dedicated folder so you can easily find it every lesson, for example:

- `Documents/DevOps/devops-demo`
- `Desktop/DevOps/devops-demo`

You should **continue working on the same project** throughout the DevOps module.

Create a new Spring Boot project locally with:

- Java 17
- Maven
- Dependency: Spring Web
- Packaging: Jar

Add one controller endpoint so we can verify the app works before we containerize it:

```java
package com.example.devopsdemo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DemoController {

    @GetMapping("/hello")
    public String hello() {
        return "DevOps demo application is running!";
    }
}
```

Run the Spring Boot application (IDE run button or terminal), then verify:

- Open: `http://localhost:8080/hello`
- You should see: `DevOps demo application is running!`

Stop the application before proceeding to Docker.

---

A sample Spring Boot Application is included in this DevOps demo project. Follow the respective steps to create a `Dockerfile` where you will use to create an image and run the image as container locally.

Step 1: Create `Dockerfile` file within the project root folder.

Step 2: Fill the file content with the following:

*Note to the instructor: please walk the learners through each line of instruction as you type it out*

```dockerfile
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

ENV PORT=8080

COPY target/*.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
```

Step 3: Create `.dockerignore` file and add `target/` to the content.

*Note: The `.dockerignore` is similar to `.gitignore` wherein it disregards any file and directory included in the file.*

Example `.dockerignore`:

```text
target/
.git
.gitignore
.idea/
.vscode/
```

To understand the syntax of the Dockerfile, see this reference [link](https://docs.docker.com/engine/reference/builder/).

Here is a quick summary. Take 5 to 10 minutes to allow learners to read through, then invite questions.

|Keyword|Purpose|
|-------|-------|
|`FROM`|The FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions.|
|`WORKDIR`|The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it.|
|`ENV`|The ENV instruction sets the environment variable <key> to the value <value>.|
|`COPY`|The COPY instruction copies new files or directories.|
|`RUN`|The RUN instruction will execute any commands in a new layer on top of the current image and commit the results.|
|`CMD`|There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect. The main purpose of a CMD is to provide defaults for an executing container. |

---

## Part 3 - Creation of Images and Running Containers

### Images and Containers

**Images** and **Containers** are some of the objects that Docker provides:

**Image** - An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. 

To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image.

**Container** - A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. 

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that aren't stored in persistent storage disappear.

### Hands-on Demonstration

We will use the CLI to build an image and run it as container. In summary, these are the commands we would touch on.

|Commands|Description|
|-|-|
|`docker image ls`| List all images that are being created|
|`docker image rm <image id>`| Remove specific image by ID|
|`docker build <options>`| Build an image from the Dockerfile.|
|`docker ps`| List all running container|
|`docker stop <container id>`| Stop a running container|
|`docker rm <container id>`|Remove a container|
|`docker run <options> <image id>`| Run an image as container|

Follow these steps to create an image and launch the container:

1. Step 1: Launch the Terminal/Powershell
2. Step 2: `cd` to the Spring Boot DevOps demo project directory
3. Step 3: (If needed) build the jar: `mvn clean package -DskipTests`
4. Step 4: Run `docker build -t "mysampleapp" .` to build an image with the repository name `mysampleapp`
5. Step 5: Run `docker image ls` to list all images. You should see a table with `IMAGE ID` as one of the column. You will need that to run image as container in the next step.
6. Step 6: Run `docker run -d -p 8080:8080 <image id>` to start the container
7. Step 7: Navigate to [http://localhost:8080/hello](http://localhost:8080/hello) to view the endpoint.

> The `-p 8080:8080` option refers to binding the host's port `8080` to the container's port `8080`. If the command is modified to `8081:8080` you are binding host's port `8081` to container's port `8080`. And the URL has to be changed to port `8081`.

### Activity 
Use the docker commands to stop the running container and deleting the container.

---

## Part 4 - Multi-stage Builds

Multi-stage builds are useful in maintaining a single Dockerfile that contains stages of builds. An example of this in Java is using a Maven build for the first stage and using that build for create a `.jar` file.

With multi-stage builds, multiple `FROM` statements can be used in the Dockerfile. Each FROM instruction can use a different base image, and each of them begins a new stage of the build. Artifacts from one stage can be copied to another, building for the final image.

```Dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS build
ENV PORT=8081
COPY . /app
WORKDIR /app
RUN mvn clean install -DskipTests

# Second stage: create a slim image
FROM eclipse-temurin:17
ENV PORT=8081
COPY --from=build /app/target/*.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### Naming build stages

By default, the stages aren't named, and can refer to by their index, starting with 0 for the first `FROM` instruction. Docker, however, can name use stages using the `AS` keyword together with the build name you want.

```Dockerfile
# The first instruction is named "build"
FROM maven:3.9.6-eclipse-temurin-17 AS build
```

After creating the name (also called alias) for the build, it can be referred to by the other stages using its name

```Dockerfile
# The build here is the artifact that is produced by the first stage
COPY --from=build /app/target/*.jar /app.jar
```

### Running multi-stage builds

Running a mult-stage build is similar to running a single stage build using the docker build command:
`docker build -t <image name> .`

### Docker logs

Running containers can be monitored using the `docker logs` command. It allows us to view log for each container. Try the command with the following options and see what the logs would show:

| Commands | Descriptions |
|-|-|
| --details | Show extra details provided to logs |
| --since | Show logs since timestamp (need to provide a timestamp value) |
| --tail | Number of lines to show from the end of the logs (need to provide n number of lines) |
| --timestamps | Show timestamps |
| --until | Show logs before a timestamp (need to provide a timestamp value) |

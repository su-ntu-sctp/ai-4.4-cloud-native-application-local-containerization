# Lesson 4.4: Cloud Native Application - Local Containerization

**Module 4: DevSecOps**  
**Duration:** 3 hours  
**Level:** Beginner

---

## Learning Objectives

By the end of this lesson, you will be able to:

1. **Explain** the concept of containerization and its benefits in modern software delivery
2. **Differentiate** between containers and virtual machines in terms of architecture and use cases
3. **Build and run** a Docker image for a Spring Boot application on a local machine
4. **Apply** multi-stage Docker builds to create optimized production-ready images

---

## Prerequisites

Before starting this lesson, ensure you have:

- Java 21 (already installed)
- Maven (already installed)
- Docker Desktop installed and running
- Your **devops-demo** Spring Boot project from Lesson 4.2

---

## Introduction

This lesson introduces containerization, which will be used throughout the entire DevOps module. You'll learn to package your Spring Boot application into a Docker container that runs consistently across different environments.

---

## Part 1 - What is Containerization (20 minutes)

Containerization is a software deployment process that bundles an application's code with all the files and libraries it needs to run on any infrastructure. 

Traditionally, to run any application on your computer, you had to install the version that matched your machine's operating system.

### What are Containers?

Containers are lightweight packages of your application code together with dependencies such as specific versions of programming language runtimes and libraries required to run your software services.

<img src="https://middleware.io/wp-content/uploads/2021/12/containerization-explained.jpg" width="50%">

### Containers vs Virtual Machines

Containers and virtual machines are deployment technologies that make applications independent from infrastructure resources.

Containers essentially are packaged application images that contain all the necessary files and dependencies to run the application.

Virtual machines, on the other hand, involves creating a guest operating system as well as virtualizing the underlying physical infrastructure.

<img src="https://s7280.pcdn.co/wp-content/uploads/2018/08/containers-vs-virtual-machines-810x413.png">

### Benefits of Containerization

Containerization can provide these benefits for applications:
- **Portability** - Allows "write once, run anywhere" by packaging the application with its dependencies
- **Efficiency** - Improves efficiency by using all available resources and minimizing overhead
- **Agility** - Allows for rapid deployment to any environment
- **Faster delivery** - Containerization can divide a large application into discrete parts using microservices
- **Improved security** - Improves security by isolation, wherein even if a container is compromised, other containers remain secure
- **Faster app startup** - Containers are lightweight so startup times are virtually instantaneous
- **Easier management** - Container images can easily be managed using Docker Desktop and Kubernetes
- **Flexibility** - Microservices can become flexible such that certain elements can be hosted on bare metal or deployed to virtual cloud environments

### Containerization Use Cases

Containerization can be used for different use cases, here are some of them: 

1. Migration from Monolithic applications to Microservices
2. Setting up and standardization of development environments
3. Infrastructure as Code
4. Hybrid cloud setups
5. Disaster Recovery

---

## Part 2 - Docker and Dockerfile (40 minutes)

For containerization, we will be using Docker.

### What is Docker?

Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run.

### When to use Docker?

Docker can be used for the following:

- **MICROSERVICES** - Build and scale distributed application architectures by taking advantage of standardized code deployments using Docker containers
- **CONTINUOUS INTEGRATION & DELIVERY** - Accelerate application delivery by standardizing environments and removing conflicts between language stacks and versions
- **DATA PROCESSING** - Provide big data processing as a service. Package data and analytics packages into portable containers that can be executed by non-technical users
- **CONTAINERS AS A SERVICE** - Build and ship distributed applications with content and infrastructure that is IT-managed and secured

### Writing Dockerfiles

Docker can build images automatically by reading the instructions from a `Dockerfile`. A `Dockerfile` is a text document that contains all the commands a user could call on the command line to create an image.

The format of a dockerfile is as follows:

```dockerfile
#comment
INSTRUCTION arguments
```

*Note: The instruction is not case-sensitive. However, convention is to make them UPPERCASE to distinguish them from arguments.*

---

### Verify Your DevOps Demo Project

For this DevOps module, you use **one Spring Boot DevOps demo project on your local machine** and reuse it for every DevOps lesson.

Make sure you have:

1. Your **devops-demo** project from Lesson 4.2
2. The `/hello` endpoint working
3. Maven can build the project: `mvn clean package -DskipTests`

If you need to verify, run the Spring Boot application and open: `http://localhost:8080/hello`

You should see: `DevOps demo application is running!`

Stop the application before proceeding to Docker.

---

### Create Dockerfile

**Step 1:** Create a file named `Dockerfile` in your project root folder (same level as `pom.xml`)

**Step 2:** Add this content:

*Note to the instructor: please walk the learners through each line of instruction as you type it out*

```dockerfile
FROM eclipse-temurin:21-jdk-alpine

WORKDIR /app

ENV PORT=8080

COPY target/*.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
```

**Step 3:** Create a `.dockerignore` file in your project root

**Step 4:** Add this content:

```text
target/classes/
target/test-classes/
target/generated-sources/
target/maven-status/
.git
.gitignore
.idea/
.vscode/
```

**Important:** Notice we exclude `target/classes/` but NOT `target/` itself. This allows Docker to copy `target/*.jar` while excluding unnecessary files.

---

### Understanding Dockerfile Instructions

Take 5 to 10 minutes to allow learners to read through, then invite questions.

|Keyword|Purpose|
|-------|-------|
|`FROM`|The FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions.|
|`WORKDIR`|The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it.|
|`ENV`|The ENV instruction sets the environment variable <key> to the value <value>.|
|`COPY`|The COPY instruction copies new files or directories.|
|`EXPOSE`|The EXPOSE instruction informs Docker that the container listens on the specified network port at runtime.|
|`RUN`|The RUN instruction will execute any commands in a new layer on top of the current image and commit the results.|
|`CMD`|There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect. The main purpose of a CMD is to provide defaults for an executing container.|

---

## Part 3 - Creation of Images and Running Containers (50 minutes)

### Images and Containers

**Images** and **Containers** are some of the objects that Docker provides:

**Image** - An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. 

To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image.

**Container** - A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. 

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that aren't stored in persistent storage disappear.

---

### Docker Commands Reference

We will use the CLI to build an image and run it as container. Here are the commands we will use:

|Commands|Description|
|-|-|
|`docker image ls`| List all images that are being created|
|`docker image rm <image id>`| Remove specific image by ID|
|`docker build <options>`| Build an image from the Dockerfile|
|`docker ps`| List all running containers|
|`docker ps -a`| List all containers (running and stopped)|
|`docker stop <container id>`| Stop a running container|
|`docker rm <container id>`|Remove a container|
|`docker run <options> <image id>`| Run an image as container|

---

### Hands-on: Build and Run Docker Container

Follow these steps to create an image and launch the container:

**Step 1:** Launch Terminal (Mac/WSL) or PowerShell (Windows)

**Step 2:** Navigate to your devops-demo project directory

```bash
cd path/to/devops-demo
```

**Step 3:** Build the JAR file

```bash
mvn clean package -DskipTests
```

**Expected output:**
```
[INFO] BUILD SUCCESS
[INFO] Total time: X seconds
```

Verify the JAR exists:
```bash
ls target/*.jar
```

**Step 4:** Build the Docker image

```bash
docker build -t mysampleapp .
```

**Expected output:**
```
[+] Building 2.3s (8/8) FINISHED
=> [1/3] FROM eclipse-temurin:21-jdk-alpine
=> [2/3] COPY target/*.jar app.jar
=> [3/3] CMD ["java", "-jar", "app.jar"]
=> => naming to docker.io/library/mysampleapp
```

**Step 5:** List Docker images

```bash
docker image ls
```

**Expected output:**
```
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
mysampleapp     latest    abc123def456   10 seconds ago   350MB
```

Copy the **IMAGE ID** - you'll need it in the next step.

**Step 6:** Run the container

```bash
docker run -d -p 8080:8080 <image-id>
```

Replace `<image-id>` with your actual IMAGE ID.

**Example:**
```bash
docker run -d -p 8080:8080 abc123def456
```

**Expected output:**
```
def789ghi012jkl345mno678pqr901stu234vwx567yza890bcd123
```

This long string is your **CONTAINER ID**.

**Explanation of flags:**
- `-d` = detached mode (runs in background)
- `-p 8080:8080` = port mapping (host:container)

> The `-p 8080:8080` option binds the host's port `8080` to the container's port `8080`. If you use `-p 8081:8080`, you bind host's port `8081` to container's port `8080`, and you'd access it at `http://localhost:8081/hello`.

**Step 7:** Verify container is running

```bash
docker ps
```

**Expected output:**
```
CONTAINER ID   IMAGE          COMMAND                  STATUS         PORTS
def789ghi012   abc123def456   "java -jar app.jar"      Up 10 seconds  0.0.0.0:8080->8080/tcp
```

**Step 8:** Test your application

Open browser and navigate to: `http://localhost:8080/hello`

**Expected response:**
```
DevOps demo application is running!
```

**Success!** Your Spring Boot app is running in a Docker container! 🎉

---

### Activity: Stop and Remove Container (10 minutes)

Practice managing Docker containers with these commands:

**1. List running containers:**
```bash
docker ps
```

**2. Stop the container:**
```bash
docker stop <container-id>
```

Use the first 4 characters of the CONTAINER ID. For example:
```bash
docker stop def7
```

**3. Verify it's stopped:**
```bash
docker ps
```

Container should no longer appear.

**4. List all containers (including stopped):**
```bash
docker ps -a
```

**5. Remove the stopped container:**
```bash
docker rm <container-id>
```

**6. Verify it's removed:**
```bash
docker ps -a
```

Container should no longer appear.

**7. (Optional) Remove the image:**
```bash
docker image rm mysampleapp
```

---

## Part 4 - Multi-stage Builds (40 minutes)

Multi-stage builds are useful in maintaining a single Dockerfile that contains stages of builds. An example of this in Java is using a Maven build for the first stage and using that build to create a `.jar` file.

With multi-stage builds, multiple `FROM` statements can be used in the Dockerfile. Each FROM instruction can use a different base image, and each of them begins a new stage of the build. Artifacts from one stage can be copied to another, building for the final image.

### Multi-stage Dockerfile

Replace your current Dockerfile content with this multi-stage version:

```dockerfile
# First stage: Build with Maven
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY . .
RUN mvn clean install -DskipTests

# Second stage: Create slim runtime image
FROM eclipse-temurin:21-jdk-alpine
ENV PORT=8080
COPY --from=build /app/target/*.jar /app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### Naming Build Stages

By default, the stages aren't named, and can be referred to by their index, starting with 0 for the first `FROM` instruction. Docker, however, can name stages using the `AS` keyword together with the build name you want.

```dockerfile
# The first instruction is named "build"
FROM maven:3.9-eclipse-temurin-21 AS build
```

After creating the name (also called alias) for the build, it can be referred to by other stages using its name:

```dockerfile
# The "build" here refers to the artifact produced by the first stage
COPY --from=build /app/target/*.jar /app.jar
```

### Benefits of Multi-stage Builds

**Smaller image size:**
- First stage includes Maven + source code (large)
- Second stage only includes JRE + JAR file (small)
- Final image is ~70% smaller

**Simpler workflow:**
- Don't need to run `mvn package` separately
- Just run `docker build` and everything happens automatically
- Maven runs inside Docker

**Single vs Multi-stage comparison:**

| Approach | You run | Docker runs | Image size |
|----------|---------|-------------|------------|
| Single-stage | `mvn package` | Copy JAR, create image | ~350MB |
| Multi-stage | `docker build` | Maven build + create image | ~350MB |

**Note:** Image sizes are similar because both use the same JDK base image. The benefit of multi-stage is the simplified workflow, not size reduction in this case.

---

### Running Multi-stage Builds

Running a multi-stage build is similar to running a single stage build:

**Step 1:** Build the image
```bash
docker build -t mysampleapp .
```

**Note:** You do NOT need to run `mvn package` first! Docker will run Maven for you.

**Step 2:** Run the container
```bash
docker run -d -p 8080:8080 mysampleapp
```

**Step 3:** Test
Open: `http://localhost:8080/hello`

---

### Docker Logs

Running containers can be monitored using the `docker logs` command. It allows us to view logs for each container.

**Basic usage:**
```bash
docker logs <container-id>
```

**Useful options:**

| Commands | Descriptions |
|-|-|
| `docker logs <container-id>` | Show all logs |
| `docker logs --details <container-id>` | Show extra details provided to logs |
| `docker logs --since 10m <container-id>` | Show logs from last 10 minutes |
| `docker logs --tail 50 <container-id>` | Show last 50 lines |
| `docker logs --timestamps <container-id>` | Show timestamps |
| `docker logs --follow <container-id>` | Follow log output (like tail -f) |

**Example:**
```bash
# See logs for your running container
docker ps  # Get container ID
docker logs def7  # View logs
```

You should see Spring Boot startup logs in the output.

---

## Summary

### What You Accomplished Today

1. ✅ Understood containerization concepts and benefits
2. ✅ Differentiated between containers and virtual machines
3. ✅ Created a Dockerfile for Spring Boot application
4. ✅ Built Docker images and ran containers locally
5. ✅ Implemented multi-stage Docker builds
6. ✅ Managed containers using Docker CLI commands

### Key Takeaways

- **Containers package applications with dependencies** - ensuring consistency across environments
- **Docker standardizes containerization** - making it easy to build, ship, and run applications
- **Multi-stage builds optimize workflows** - combining build and runtime in one Dockerfile
- **Docker CLI provides powerful commands** - for managing images and containers

---

## Troubleshooting Guide

### Issue 1: "COPY failed: file not found"

**Cause:** JAR file doesn't exist or .dockerignore is blocking it

**Solution:**
1. Verify JAR exists: `ls target/*.jar`
2. If missing, run: `mvn clean package -DskipTests`
3. Check .dockerignore doesn't have `target/` (should only have `target/classes/` etc.)

---

### Issue 2: "Cannot connect to Docker daemon"

**Cause:** Docker Desktop not running

**Solution:**
1. Open Docker Desktop
2. Wait for it to start completely
3. Try command again

---

### Issue 3: Port 8080 already in use

**Cause:** Another application is using port 8080

**Solution:**
Use a different host port:
```bash
docker run -d -p 8081:8080 <image-id>
```
Then access: `http://localhost:8081/hello`

---

### Issue 4: Container starts but endpoint returns 404

**Cause:** Application took time to start

**Solution:**
1. Wait 10-20 seconds after starting container
2. Check logs: `docker logs <container-id>`
3. Look for "Started Application in X seconds"
4. Then try accessing endpoint again

---

## Additional Resources

### Docker Documentation
- [Docker Overview](https://docs.docker.com/get-started/overview/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Multi-stage Builds](https://docs.docker.com/build/building/multi-stage/)
- [Docker CLI Commands](https://docs.docker.com/engine/reference/commandline/cli/)

### Video Tutorials
- [Docker Tutorial for Beginners](https://www.youtube.com/results?search_query=docker+tutorial+beginners)
- [Dockerizing Spring Boot Application](https://www.youtube.com/results?search_query=docker+spring+boot+tutorial)

---

**End of Lesson 4.4**

**Congratulations!** You've successfully containerized your Spring Boot application using Docker. This is a fundamental DevOps skill that you'll use throughout the remaining lessons! 🎉

**Next Lesson:** Lesson 4.5 - Docker Compose (Multi-container Applications)
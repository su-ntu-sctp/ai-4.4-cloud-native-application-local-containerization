# 4.4 Self Studies

**Estimated Preparation Time:** 70 minutes

---

## Task 1 — Docker Basics (25 minutes)

Watch the following video on Docker fundamentals:

- 📹 [Docker Tutorial for Beginners — https://www.youtube.com/watch?v=DQdB7wFEygo]

While watching, refer to **lesson.md Part 1 and Part 2** and pay attention to:
- What Docker is and why it is used in software development
- The difference between containers and virtual machines
- How Docker images and containers relate to each other
- The basic Docker CLI commands used to build, run, and manage containers

**Guiding Questions:**
1. In your own words, what is the difference between a Docker image and a Docker container?
2. Why is Docker preferred over virtual machines for application deployment?
3. Which Docker CLI commands would you use to check if a container is running and to stop it?

---

## Task 2 — Dockerizing a Spring Boot Application (25 minutes)

Watch the following video on containerizing a Spring Boot application:

- 📹 [Dockerize Your Spring Boot Application — https://www.youtube.com/watch?v=05ADPwAmiAs]

While watching, refer to **lesson.md Part 2 and Part 3** and pay attention to:
- How a Dockerfile is structured for a Spring Boot application
- What each Dockerfile instruction does (FROM, WORKDIR, COPY, EXPOSE, CMD)
- How the JAR file is built and copied into the Docker image
- How port mapping works when running a container

**Guiding Questions:**
1. Why do we run `mvn clean package` before building the Docker image in a single-stage build?
2. What does `-p 8080:8080` mean in the `docker run` command?
3. What would happen if you forgot the `EXPOSE` instruction in your Dockerfile?

---

## Task 3 — Multi-stage Builds (20 minutes)

No video for this task. Refer to **lesson.md Part 4** and read through the multi-stage Dockerfile section carefully, then try the following:

1. Update your Dockerfile to use the multi-stage build version from the lesson
2. Build the image without running `mvn package` first:
```bash
docker build -t mysampleapp-multistage .
```
3. Run the container and verify the endpoint works:
```bash
docker run -d -p 8080:8080 mysampleapp-multistage
```
4. Check the logs to confirm the application started successfully:
```bash
docker logs <container-id>
```

**Guiding Questions:**
1. What is the main advantage of a multi-stage build over a single-stage build?
2. What does `COPY --from=build` do in the second stage of the Dockerfile?
3. Why is the final image in a multi-stage build smaller than the build stage?

---

## Active Engagement Strategies

- Pause the first video whenever a Docker CLI command is shown and try running it yourself
- After watching Task 2, close the video and try to write the Dockerfile from memory before checking
- For Task 3, do not copy-paste the Dockerfile — type it out manually to build familiarity with the syntax

---

## Additional Reading Material

- [Docker Overview — Docker Docs](https://docs.docker.com/get-started/overview/)
- [Dockerfile Reference — Docker Docs](https://docs.docker.com/engine/reference/builder/)
- [Multi-stage Builds — Docker Docs](https://docs.docker.com/build/building/multi-stage/)
- [Containerization Explained — IBM](https://www.ibm.com/topics/containerization)
# Assignment (Optional)

## Brief

Practice Docker containerization by creating a Dockerfile and building Docker images for a Spring Boot application.

1. **Create and Run a Simple Docker Container**
   - Use your existing Spring Boot project (e.g., devops-demo or any project with a REST endpoint)
   - Ensure your project builds successfully with Maven:
```bash
     mvn clean package -DskipTests
```
   - Create a `Dockerfile` in the project root with the following:
     - Use `eclipse-temurin:21-jdk-alpine` as the base image
     - Set working directory to `/app`
     - Set environment variable `PORT=8080`
     - Copy the JAR file from `target/` to `app.jar`
     - Expose port 8080
     - Add CMD instruction to run the JAR file with `java -jar app.jar`
   - Create a `.dockerignore` file to exclude:
     - `target/classes/`
     - `target/test-classes/`
     - `.git`
     - `.gitignore`
   - Build the Docker image:
```bash
     docker build -t myapp .
```
   - Run the container:
```bash
     docker run -d -p 8080:8080 myapp
```
   - Test your application by accessing the endpoint in your browser
   - Verify the container is running with `docker ps`
   - Take a screenshot showing:
     - Your running container (`docker ps` output)
     - Your application responding successfully in the browser
   - Stop and remove the container when done

2. **Implement Multi-stage Docker Build**
   - Update your Dockerfile to use multi-stage builds:
     - **First stage (build stage)**:
       - Use `maven:3.9-eclipse-temurin-21` as base image
       - Name this stage "build" using `AS build`
       - Set working directory to `/app`
       - Copy all project files
       - Run `mvn clean install -DskipTests`
     - **Second stage (runtime stage)**:
       - Use `eclipse-temurin:21-jdk-alpine` as base image
       - Set environment variable `PORT=8080`
       - Copy the JAR file from the build stage using `COPY --from=build`
       - Expose port 8080
       - Add ENTRYPOINT instruction to run the JAR file
   - Build the multi-stage Docker image (no need to run `mvn package` separately):
```bash
     docker build -t myapp-multistage .
```
   - Run the container:
```bash
     docker run -d -p 8080:8080 myapp-multistage
```
   - View the container logs to verify it started successfully:
```bash
     docker logs <container-id>
```
   - Test the application endpoint
   - Write a brief explanation (3-4 sentences) describing:
     - The difference between single-stage and multi-stage builds
     - The benefit of using multi-stage builds
     - What happens in each stage of your Dockerfile

## Submission (Optional)

- Submit the URL of the GitHub Repository that contains your work.

## References
- Java: https://docs.oracle.com/javase/
- Spring Boot: https://docs.spring.io/spring-boot/docs/current/reference/html/
- PostgreSQL: https://www.postgresql.org/docs/
- OWASP: https://cheatsheetseries.owasp.org/
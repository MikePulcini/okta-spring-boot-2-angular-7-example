# Stage 1: Build via maven
FROM maven:3.5.0-jdk-8-alpine AS build

RUN mkdir -p /usr/server
WORKDIR /usr/server

# Copy the actual code
COPY ./ .

# Compile the webapp (downloading all dependencies) and build fat JAR
RUN mvn compile package
RUN mv target/demo-0.0.1-SNAPSHOT.jar target/app.jar

# Stage 2: Execute webapp
FROM openjdk:8-jre-alpine

COPY --from=build /usr/server/target/app.jar /usr/server/app.jar

CMD ["java", "-jar", "/usr/server/app.jar"]
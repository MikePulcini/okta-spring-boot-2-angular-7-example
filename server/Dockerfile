FROM maven:3.5.0-jdk-8-alpine

RUN mkdir -p /usr/server
WORKDIR /usr/server

# Copy the actual code
COPY ./ .

# Run spring-boot:run to build, package, and run the webapp
# TODO: Optimize this so we don't have to rebuild the JAR and redownload all dependencies each time
CMD ["mvn", "spring-boot:run"]

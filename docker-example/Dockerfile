# Start with a base image containing Java runtime
FROM dperezcabrera/openjdk11-alpine

# Add Maintainer Info
MAINTAINER Zeeshan Alam Khan <zeeshan.maths@gmail.com>

# Add a volume pointing to /tmp
VOLUME /tmp

# Make port 8080 available to the world outside this container
EXPOSE 8080

# The application's jar file
ARG JAR_FILE=target/docker-example.jar

# Add the application's jar to the container
ADD ${JAR_FILE} docker-example.jar

# Run the jar file 
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/docker-example.jar"]

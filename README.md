# spring-boot-docker
How to build a docker image for running a spring boot application using the dockerfile-maven-plugin from Spotify, with Docker Toolbox.

We need to have a Dockerfile — for the purpose of demonstration there’s a Dockerfile. Add Dockerfile into our root directory alongside our pom.xml.

Dockerfile
------------------------------------------------------------

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

Next we will add some configuration into the pom.xml, specifically repository tag and an argument for the JAR_FILE which you can see we reference in the Dockerfile above.

spotify dockerfile maven plugin
--------------------------------------------------------------

     <plugin>
				<groupId>com.spotify</groupId>
				<artifactId>dockerfile-maven-plugin</artifactId>
				<version>1.4.13</version>
				<configuration>
					<repository>zeeshanalamkhan/docker-example</repository>
					<tag>${project.version}</tag>
					<dockerHost>https://192.168.99.100:2376</dockerHost>
					<dockerCertPath>C:/Users/<Your User name>/.docker/machine/machines/default
					</dockerCertPath>
					<buildArgs>
						<JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
					</buildArgs>
				</configuration>
				<executions>
					<execution>
						<id>default</id>
						<phase>install</phase>
						<goals>
							<goal>build</goal>
							<goal>push</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
      
      
Now run the project as maven build ->goal -> clean package dockerfile:build
On completion we should see a message indicating the Docker image has been successfully created.

Once built, we can run our new Docker image as. 
docker run -p 8080:8080 docker-example:0.0.1-SNAPSHOT

and we can access our application from web browser or postman as
192.168.99.102:8080/hello/{any name}
 


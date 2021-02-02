Simple Spring Boot application to learn how to use Docker.
It has only one endpoint ("/hello"), which return String with
"hello" and appended environmental variable added with
CMD at Dockerfile.

Project also contains Dockerfile with configuration to
make image with JDK 8 Alpine. 


```
FROM openjdk:8u191-jdk-alpine3.9
ADD target/spring-docker-0.0.1-SNAPSHOT.jar .
EXPOSE 8000
CMD java -jar spring-docker-0.0.1-SNAPSHOT.jar --envname=prod
```

We start with "FROM" and we take image from DockerHub we
want to append to our image. In this case we want to use
JDK 8 Alpine. I used "mvn clean compile package" to create JAR file inside target folder. In the next line, I used 
"ADD" to add that JAR file into the image. Next line
defines the port that is exposed, that means, the port
used to communicate with container. In our case it's 8000.
The final line start with "CMD" and is responsible for 
commands executed at the start of application.

In the application.properties we have two lines:

```
envname=
server.port=${PORT:8080}
```

envname is environmental variable, that is changed at the start "--envname=prod" in CMD in Dockerfile. server.port is
used to know which port Spring app uses.

App is deployed at heroku. It can be found here: [Hello endpoint](https://docker-spring-test.herokuapp.com/hello "App on Heroku")


To create an image I used:
```
docker build -f Dockerfile -t dockerSpring:v1 .
```

Then, to run an image, I used:
```
docker run -p 8000:8080 410
```
Where 410 is first three characters of Image ID.

One more important thing, while deploying to Heroku, I added '-a projectname', it was obligatory, but not written on Heroku page:
```
heroku container:release web -a docker-spring-test
```

Other useful Docker commands:
```
docker ps - currenctly running containers
docker stop abc - stop container that starts with abc
```

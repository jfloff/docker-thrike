# Thrike (*T*omcat, T*hri*ft, JD*K* and Gradl*e*)

[![Docker Stars](https://img.shields.io/docker/stars/jfloff/https://github.com/jfloff/docker-thrike.svg)][hub]
[![Docker Pulls](https://img.shields.io/docker/pulls/jfloff/docker-thrike.svg)][hub]

[hub]: https://hub.docker.com/r/jfloff/docker-thrike/

Yikes, what an ugly name! Well, this is a docker image for projects that rely on Tomcat and Thrift (for me it was a [microservice-based Spring Boot with Thrift](https://github.com/ExampleDriven/spring-boot-thrift-example)) and also need JDK and Gradle. Is this odly specific, yes. But its easier to maintain this way. This has a mashup of several other images, namely:
- [Tomcat](https://github.com/docker-library/tomcat/blob/master/8.5/jre8/Dockerfile)
- [Thrift](https://github.com/ahawkins/docker-thrift/blob/master/0.10/Dockerfile)
- [JDK](https://github.com/docker-library/openjdk/blob/master/8-jdk/alpine/Dockerfile)
- [Gradle](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile)


## Supported tags
* **`8.5`,`latest`** *([Dockerfile](https://github.com/jfloff/docker-thrike/blob/master/8.5/Dockerfile))*

Images are tagged with the JDK version numbers. If you need any other version, please **feel free to open a PR**.


### Usage
If you are not familiar with Docker, or just forget the commands all the time like me, here is a rundown:
```
# run linking to the admin panel and main page
docker run --rm -ti jfloff/thrike:latest

# Use as base in another Dockerfile
FROM jfloff/thrike:latest

# build from this repo's Dockerfile (if you download it)
docker build --rm -t jfloff/thrike .
```


### License
The code in this repository, unless otherwise noted, is MIT licensed. See the `LICENSE` file in this repository.
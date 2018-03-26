# Thrike

[![Docker Stars](https://img.shields.io/docker/stars/jfloff/thrike.svg)][hub]
[![Docker Pulls](https://img.shields.io/docker/pulls/jfloff/thrike.svg)][hub]

[hub]: https://hub.docker.com/r/jfloff/thrike/

This is a docker image for projects that use **Tomcat for building Microservices**. We also added standard RPC frameworks for easier development. Here is a list of things installed:
- [Tomcat](http://tomcat.apache.org/) ([based on](https://github.com/docker-library/tomcat/blob/master/8.5/jre8/Dockerfile))
- [Thrift](https://thrift.apache.org/) ([based on](https://github.com/ahawkins/docker-thrift/blob/master/0.10/Dockerfile))
- [gRPC](https://grpc.io/) ([based on](https://github.com/jfloff/grpc-java-sbt/blob/master/Dockerfile))
- [Gradle](http://openjdk.java.net/) ([based on](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile))
- [OpenJDK](http://openjdk.java.net/) ([based on](https://github.com/docker-library/openjdk/blob/master/8-jdk/alpine/Dockerfile))


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

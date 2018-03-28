# Thrike

[![Docker Stars](https://img.shields.io/docker/stars/jfloff/thrike.svg)][hub]
[![Docker Pulls](https://img.shields.io/docker/pulls/jfloff/thrike.svg)][hub]

[hub]: https://hub.docker.com/r/jfloff/thrike/

This is a docker image for projects that use **Tomcat for building Microservices**. We also added standard RPC frameworks for easier development. Here is a list of things installed:

- **[Tomcat](http://tomcat.apache.org/) (v8.5)**

    Apache Tomcat webserver is the base of this image ([original docker image](https://github.com/docker-library/tomcat/blob/master/8.5/jre8/Dockerfile)).

- **[Gradle](http://openjdk.java.net/) (v4.6)**

    Gradle build system. Dockerfile commands based on [this Docker image](https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile)).

- **[Thrift](https://thrift.apache.org/) (v0.11.0)**

    Apache Thrift for RPC communications between microservices. Dockerfile commands based on [this Docker image](https://github.com/ahawkins/docker-thrift/blob/master/0.10/Dockerfile).

- **[Google Protocol Buffers](https://developers.google.com/protocol-buffers/) (v.3.5.1)**

    Also know as [protobuf](https://github.com/google/protobuf), this is a protocol for serializing structured data. The `protoc` compiler is available for your use. Dockerfile commands based on [this Docker image](https://github.com/jfloff/grpc-java-sbt).

- **[gRPC](https://grpc.io/) (v.1.10.0)**

    Java bindings for Google's RPC framework. We also added the `grpc-java` plugin built from source (downloading from [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22protoc-gen-grpc-java%22) is is also possible if compatible with your OS, but Alpine Linux doesn't have binaries yet). Dockerfile instructions based on the [grpc-java build instructions](https://github.com/grpc/grpc-java/tree/master/compiler) and [this Docker image](https://github.com/jfloff/grpc-java-sbt).


## Supported tags
* **`8.5`,`latest`** *([Dockerfile](https://github.com/jfloff/docker-thrike/blob/master/8.5/Dockerfile))*

Images are tagged with the Tomcat version numbers. If you need any other version, please **feel free to open a PR**.


## Environment variables
| Variable | Description | Value |
| -------- | ----------- | ------- |
| PROTOC_HOME | Path for the protobuf compiler | `/usr/bin/protoc` |
| GRPC_JAVA_PLUGIN_HOME | Path to the gRPC Java plugin | `/usr/local/bin/protoc-gen-grpc-java` |
| JAVA_HOME | Java home | `/usr/lib/jvm/java-1.8-openjdk` |
| GRADLE_HOME | Gradle home | `/usr/local/lib/gradle` |
| GRADLE_VERSION | Current Gradle version | `4.6` |
| PROTOBUF_VERSION | Current Protobuf version | `3.5.1` |
| GRPC_JAVA_VERSION | Current gRPC Java version | `1.10.0` |
| APACHE_THRIFT_VERSION | Current Thrift version | `0.11.0` |
| APK_GLIBC_VERSION | Current Alpine GLIBC version | `2.27-r0` |


## Usage
If you are not familiar with Docker, or just forget the commands all the time like me, here is a rundown:
```
# run linking to the admin panel and main page
docker run --rm -ti jfloff/thrike:latest

# Use as base in another Dockerfile
FROM jfloff/thrike:8.5

# build from this repo's Dockerfile (if you fork it)
docker build --rm -t jfloff/thrike .
```


## License
The code in this repository, unless otherwise noted, is MIT licensed. See the `LICENSE` file in this repository.

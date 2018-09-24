FROM tomcat:8.5-jre8-alpine

LABEL maintainer="jfloff@inesc-id.pt"
LABEL maintainer="duartercorreia@tecnico.ulisboa.pt"

ENV \
	# VERSIONS
	GRADLE_VERSION=4.6 \
	APK_GLIBC_VERSION=2.28-r0 \
	APACHE_THRIFT_VERSION=0.11.0 \
	PROTOBUF_VERSION=3.5.1 \
	GRPC_JAVA_VERSION=1.10.0 \
	# HOMES
	JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk \
	GRADLE_HOME=/usr/local/lib/gradle \
	PROTOC_HOME=/usr/bin/protoc \
	GRPC_JAVA_PLUGIN_HOME=/usr/local/bin/protoc-gen-grpc-java

RUN set -ex ;\
  	apk add --no-cache \
	  	# for glibc
		wget \
		# jdk
		openjdk8="$JAVA_ALPINE_VERSION" \
		apache-ant \
		bash \
		# so we are able to do rpc endpoint tests
		openssl \
		curl \
		nmap \
		# for debugging
		vim \
		iperf3 \
		# to get fonts in awt to work
		# https://github.com/docker-library/openjdk/issues/73
		fontconfig \
		ttf-dejavu \
		;\
	# install glibc using: https://github.com/sgerrand/alpine-pkg-glibc
	wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub ;\
	wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${APK_GLIBC_VERSION}/glibc-${APK_GLIBC_VERSION}.apk ;\
	# add build deps to be removed after
	apk add --no-cache --virtual .build-deps \
		unzip \
		libstdc++ \
		ca-certificates \
		flex \
		automake \
		autoconf \
		libtool \
		pkgconf \
		bison \
		libssl1.0 \
		libevent-dev \
		gcc \
		g++ \
		make \
		glibc-${APK_GLIBC_VERSION}.apk \
		;\
	rm -rf /var/cache/apk/*

###################
# Install Gradle
# Based on: https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile
#
RUN set -ex ;\
	wget -O gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" ;\
	unzip gradle.zip ;\
	rm gradle.zip ;\
	mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}" ;\
	ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle ;\
	# disable daemon since we use containers no point in using this
	mkdir -p /root/.gradle/ ;\
	echo "org.gradle.daemon=false" >> /root/.gradle/gradle.properties

###################
# Install Apache Thrift
# Based on: https://github.com/ahawkins/docker-thrift/blob/master/0.11/Dockerfile
#
RUN set -ex ;\
	wget http://www-eu.apache.org/dist/thrift/${APACHE_THRIFT_VERSION}/thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	tar -xvf thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	rm thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	cd thrift-${APACHE_THRIFT_VERSION}/ ;\
	./configure --without-python --without-cpp ;\
	make && make install ;\
	cd .. && rm -rf thrift-${APACHE_THRIFT_VERSION}

###################
# Install PROTOBUF
# Only works for 3.0.0 and up
#
RUN set -ex ;\
	# Install PROTOBUF
	# Only works for 3.0.0 and up
	curl -#sL "https://github.com/google/protobuf/releases/download/v$PROTOBUF_VERSION/protobuf-java-$PROTOBUF_VERSION.tar.gz" | tar -xz -C /usr/local ;\
    cd /usr/local/protobuf-$PROTOBUF_VERSION && ./configure --prefix=/usr ;\
    make && make install ;\
    rm -rf /usr/local/protobuf-$PROTOBUF_VERSION

###################
# Install GRPC JAVA
RUN set -ex ;\
    curl -#sL "https://github.com/grpc/grpc-java/archive/v$GRPC_JAVA_VERSION.tar.gz" | tar -xz -C /usr/local ;\
    cd /usr/local/grpc-java-$GRPC_JAVA_VERSION/compiler && ../gradlew java_pluginExecutable ;\
    cp ./build/exe/java_plugin/protoc-gen-grpc-java /usr/local/bin/protoc-gen-grpc-java ;\
    rm -rf /usr/local/grpc-java-$GRPC_JAVA_VERSION

###################
# Add extras and cleanup
#
RUN	set -ex ;\
	# add wait-for script to order docker-compose startup
	wget https://raw.githubusercontent.com/eficode/wait-for/master/wait-for -O /usr/bin/wait-for && chmod +x /usr/bin/wait-for ;\
	# allow all users to access manager in tomcat
	# WARNING: THIS SETTING SHOULD BE CHANGED FOR PRODUCTION
	sed -i 's/allow="127\\\.\\d+\\\.\\d+\\\.\\d+|::1|0:0:0:0:0:0:0:1"/allow="^\.*$"/g' $CATALINA_HOME/webapps/manager/META-INF/context.xml ;\
	sed -i '/<\/tomcat-users>/i<role rolename="manager-gui"/><user username="admin" password="admin" roles="manager-gui"\/>' $CATALINA_HOME/conf/tomcat-users.xml ;\
	# remove pending deps
    apk del --no-cache --purge .build-deps ;\
    rm -rf /var/cache/apk/*

###################
# Startup
#
# standard tomcat port
EXPOSE 8080 6565
CMD catalina.sh run
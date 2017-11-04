FROM tomcat:8-jre8-alpine

LABEL maintainer="jfloff@inesc-id.pt"
LABEL maintainer="duartercorreia@tecnico.ulisboa.pt"

###################
# JDK FROM: https://github.com/docker-library/openjdk/blob/master/8-jdk/alpine/Dockerfile
# GRADLE FROM: https://github.com/keeganwitt/docker-gradle/blob/master/jdk8-alpine/Dockerfile
#

ENV \
	# VERSIONS
	GRADLE_VERSION=4.0.1 \
	APK_GLIBC_VERSION=2.26-r0 \
	APACHE_THRIFT_VERSION=0.9.3 \
	# HOMES
	JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk \
	GRADLE_HOME=/usr/local/lib/gradle

ARG GRADLE_DOWNLOAD_SHA256=d717e46200d1359893f891dab047fdab98784143ac76861b53c50dbd03b44fd4

RUN set -ex ;\
  	apk add --no-cache \
		# jdk
		openjdk8="$JAVA_ALPINE_VERSION" \
		apache-ant \
		bash \
		openssl \
		;\
	# install glibc using: https://github.com/sgerrand/alpine-pkg-glibc
	wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub ;\
	wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${APK_GLIBC_VERSION}/glibc-${APK_GLIBC_VERSION}.apk ;\
	# add build deps to be removed after
	apk add --no-cache --virtual .build-deps \
		unzip \
		# gradle
		libstdc++ \
		ca-certificates \
		# thrift
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
		glibc-${APK_GLIBC_VERSION}.apk

###################
# Install Gradle
#
RUN set -ex ;\
	wget -O gradle.zip "https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" ;\
	echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum -c - ;\
	unzip gradle.zip ;\
	rm gradle.zip ;\
	mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}" ;\
	ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle ;\
	# disable daemon since we use containers no point in using this
	mkdir -p /root/.gradle/ ;\
	echo "org.gradle.daemon=false" >> /root/.gradle/gradle.properties

###################
# Install Apache Thrift
#
RUN set -ex ;\
	wget http://www-eu.apache.org/dist/thrift/${APACHE_THRIFT_VERSION}/thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	tar -xvf thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	rm thrift-${APACHE_THRIFT_VERSION}.tar.gz ;\
	cd thrift-${APACHE_THRIFT_VERSION}/ ;\
	./configure ;\
	make && make install ;\
	cd .. && rm -rf thrift-${APACHE_THRIFT_VERSION} ;\

###################
# Cleanup
#
	apk del .build-deps

CMD catalina.sh run
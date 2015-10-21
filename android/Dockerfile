#
# Android SDK Dockerfile
#
# https://github.com/uphyca/dockerfile
#

# Pull small base image.
FROM debian:wheezy


MAINTAINER Sosuke Masui "masui@uphyca.com"


# Install dependencies.
# https://developer.android.com/intl/ja/sdk/installing/index.html?pkg=tools#UbuntuTrouble
ENV DEBIAN_FRONTEND=noninteractive \
	BUILD_PACKAGES="curl" \
	RUNTIME_PACKAGES="git-core" \
	JAVA_DEPENDENCIES="libc6:i386 libstdc++6:i386 zlib1g:i386 libncurses5:i386"
RUN dpkg --add-architecture i386 && \
	apt-get update -y && \
	apt-get install -y --force-yes --no-install-recommends $BUILD_PACKAGES $RUNTIME_PACKAGES $JAVA_DEPENDENCIES && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*


# Install Oracle Java8.
# http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
	echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
	echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list && \
	apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
	apt-get update -y && \
	apt-get install  -y --force-yes --no-install-recommends oracle-java8-installer oracle-java8-set-default && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/* /var/cache/oracle-jdk8-installer
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle


# Install Android SDK.
ENV ANDROID_SDK_URL=http://dl.google.com/android/android-sdk_r24.4-linux.tgz \
	ANDROID_HOME=/usr/local/android-sdk-linux \
	ANDROID_SDK=/usr/local/android-sdk-linux
RUN curl -L ${ANDROID_SDK_URL} | tar --no-same-owner -xz -C /usr/local
ENV PATH $ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH


# Install Android packages.
ENV ANDROID_SDK_COMPONENTS platform-tools,build-tools-23.0.1,android-23,extra-android-m2repository,extra-google-m2repository
RUN echo y | android update sdk --no-ui --all --filter "${ANDROID_SDK_COMPONENTS}"


# Install Gradle.
RUN echo "deb http://ppa.launchpad.net/cwchien/gradle/ubuntu trusty main" | tee /etc/apt/sources.list.d/cwchien-gradle.list && \
	echo "deb-src http://ppa.launchpad.net/cwchien/gradle/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/cwchien-gradle.list && \
	apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
	apt-get update -y && \
	apt-get install  -y --force-yes --no-install-recommends gradle-2.8 && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*
ENV GRADLE_HOME=/usr/lib/gradle/2.8/ \
	TERM=dumb
#
# Android SDK Dockerfile
#
# https://github.com/uphyca/dockerfiles/android
#

# Pull small base image.
FROM debian:wheezy


MAINTAINER Sosuke Masui "masui@uphyca.com"


ENV DEBIAN_FRONTEND noninteractive


# Install Oracle Java8.
# http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html
# https://developer.android.com/intl/ja/sdk/installing/index.html?pkg=tools#UbuntuTrouble
# http://stackoverflow.com/questions/2710499/android-sdk-on-a-64-bit-linux-machine
ENV ANDROID_SDK_DEPENDENCIES="libc6:i386 libstdc++6:i386 zlib1g:i386 libncurses5:i386 ia32-libs" \
	JAVA_PACKAGE_VERSION=8u66+8u65arm-1~webupd8~1
RUN	echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
	echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
	echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list && \
	apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
	dpkg --add-architecture i386 && \
	apt-get update -y && \
	apt-get install  -y --force-yes --no-install-recommends $ANDROID_SDK_DEPENDENCIES oracle-java8-installer=$JAVA_PACKAGE_VERSION oracle-java8-set-default=$JAVA_PACKAGE_VERSION && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/* /var/cache/oracle-jdk8-installer
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle


# Install Gradle.
RUN echo "deb http://ppa.launchpad.net/cwchien/gradle/ubuntu trusty main" | tee /etc/apt/sources.list.d/cwchien-gradle.list && \
	echo "deb-src http://ppa.launchpad.net/cwchien/gradle/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/cwchien-gradle.list && \
	apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
	apt-get update -y && \
	apt-get install  -y --force-yes --no-install-recommends gradle-2.8 && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*
ENV GRADLE_HOME=/usr/lib/gradle/2.8/ \
	TERM=dumb


# Install Android SDK.
ENV ANDROID_SDK_URL=http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz \
	ANDROID_HOME=/usr/local/android-sdk-linux \
	ANDROID_SDK=/usr/local/android-sdk-linux
RUN apt-get update -y && \
	apt-get install -y --force-yes --no-install-recommends curl && \
	curl -L ${ANDROID_SDK_URL} | tar --no-same-owner -xz -C /usr/local && \
	apt-get autoremove -y --purge curl && \
	apt-get clean && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*
ENV PATH $ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH


# Install Android packages.
ENV ANDROID_SDK_COMPONENTS platform-tools,build-tools-23.0.2,android-23,extra-android-m2repository,extra-google-m2repository
RUN echo y | android update sdk --no-ui --all --filter "${ANDROID_SDK_COMPONENTS}"


ENV DEBIAN_FRONTEND dialog

#
# Android Emulator Dockerfile
#
# https://github.com/uphyca/dockerfiles/android-emulator
#

# Pull small base image.
FROM uphyca/android


MAINTAINER Sosuke Masui "masui@uphyca.com"


ENV DEBIAN_FRONTEND noninteractive


# Install Android packages.
ENV ANDROID_SDK_COMPONENTS sys-img-armeabi-v7a-android-23
RUN echo y | android update sdk --no-ui --all --filter "${ANDROID_SDK_COMPONENTS}"


ADD android-wait-for-emulator /usr/local/bin/
RUN chmod +x /usr/local/bin/android-wait-for-emulator

RUN echo no | android create avd \
	--force \
	--device "Nexus 5" \
	--name "my_android" \
	--target android-23 \
	--abi armeabi-v7a \
	--sdcard 512M


ENV DEBIAN_FRONTEND dialog


#CMD emulator -avd my_android -no-skin -no-audio -no-window & && \
#	android-wait-for-emulator

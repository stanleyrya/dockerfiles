FROM anapsix/alpine-java
MAINTAINER Ryan Stanley <ryan.stanley@alum.cs.umass.edu>

ARG GPU=intel
ARG JAR_PATH=http://s3.amazonaws.com/Minecraft.Download/launcher/Minecraft.jar

RUN apk --no-cache add \
	eudev-libs \
	libxcursor \
	libxtst \
	mesa-dri-$GPU \
	mesa-gl \
	sox \
	xrandr

ADD $JAR_PATH /minecraft/

WORKDIR /minecraft

CMD java -jar Minecraft.jar

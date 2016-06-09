# Run Minecraft client in an Alpine container
#
#	docker run --rm \
#				--net host \
#				-v /tmp/.X11-unix:/tmp/.X11-unix \				# mount the X11 socket
#				-v /dev/shm:/dev/shm \										# increases performance
#				-v $PWD/minecraft-data:/root/.minecraft \	# save state (and don't redownload jars)
#				-e DISPLAY=unix$DISPLAY \
#				--device /dev/dri \												# share GPU
#				--device /dev/snd \												# share sound
#				--name minecraft \
#				minecraft
#

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

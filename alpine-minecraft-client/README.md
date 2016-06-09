# Minecraft in an Alpine Docker Container!

## Build Arguments
```
GPU: The type of GPU your computer has.
  - intel (default)
  - nouveau (nvidia)

JAR_PATH: Direct path to the Minecraft.jar file. Defaults to online URL.
```

## Run
```
# make sure x11 is shared
xhost +

# build
docker build -t minecraft .

# run
docker run --rm \
  --net host \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v /dev/shm:/dev/shm \
  -v $PWD/minecraft-data:/root/.minecraft \
  -e DISPLAY=unix$DISPLAY \
  --device /dev/dri \
  --device /dev/snd \
  --name minecraft \
  minecraft
```

## Notes
### Testing GPU
It may be beneficial to run tests to see if the GPU is setup properly in docker. Here are some useful commands:
```
# connect to a running container with /bin/sh
docker exec -it minecraft /bin/sh

# install glxgears, etc.
apk --no-cache add mesa-demos

# various tests
glxgears
glxinfo | grep render

# exit from an attached container
exit
```

### VMware + VirtualBox
It might be possible to run this image in a virtual machine with some modifications. To get started, change `mesa-dri-$GPU` in the Dockerfile to `xf86-video-vmware` and see what happens.

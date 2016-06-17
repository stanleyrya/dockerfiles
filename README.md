# dockerfiles

### Troubleshooting
###### "Help! Docker isn't connected to the internet!"
You most likely are running docker on Mac or Windows and have recently switched internet connections. When you switch internet connections while your docker-machine is still running, it looses it's connection. Assuming your docker-machine is called default, run:
```
docker-machine restart default
```

###### "Help! When I try to build with docker, I get a `no space left on device` error!"
After a while docker can become cluttered with old containers, images, and volumes. You can remove some manually to get unblocked, or you can use some of the following commands to remove them in bulk. Be careful not to remove anything important!
```
# delete stopped containers
docker ps -a | awk '/Exited/ {print $1}' | xargs docker rm -v

# remove dangling images
docker rmi $(docker images -f "dangling=true" -q)

# remove all images
docker rmi $(docker images -q)

# remove dangling volumes
docker volume rm $(docker volume ls -qf dangling=true)
```

###### "Help! My docker container does not have the correct time!"
Docker containers are not guaranteed to have the same timezone as the host machine. Normally this is not an issue, however when dealing with cronjobs this could lead to seemingly erratic behavior. Luckily there is an easy fix, simply share either `/etc/timezone` or `/etc/localtime` with the container as a read-only volume. For projects that use an Alpine base image make sure to share `/etc/localtime` as `/etc/timezone` does not exist. For example:
```
docker run -v /etc/localtime:/etc/localtime:ro image
```

#### Node.js
###### "Help! When I try to build with docker, I get a `cross-device link not permitted` error!"
The most common cause of this problem is due to node modules being copied to your docker container from your host machine before running `npm install`. You can either manually remove the node modules before building with docker, or create a `.dockerignore` file with the following contents:
```
# Node.js dependency directory
node_modules
```

#### Java
###### "Help! My docker container hangs!"
This issue occurs when the JVM inside the docker container does not have enough memory. You can try adding the maximum memory allocation tag to your java command to see if that solves the problem. For example, if you would like 512mb of memory, try adding `-Xmx512m`.

On Mac OS X or Windows? You may also need to increase your docker-machine's memory. You can use the following commands to do so, however be warned that this will remove all existing containers and volumes.
```
docker-machine rm default
docker-machine create -d virtualbox --virtualbox-memory 4096 --virtualbox-cpu-count 2 default
```

If you need a fix and can't remove your old docker-machine, just create a new one with a different name and switch to it using `docker eval [docker-machine name]`.

You can check your docker-machine's current settings here if you are a Mac OS X user:
```
~/.docker/machine/machines/default/config.json
```

#### Docker Compose and Git Submodules
###### "Help! docker-compose says files are missing!"
9 times out of 10 this is due to not updating your git submodules when changing commits. Assuming you did a recursive clone of the parent repo, just run:
```
git submodule update --recursive
```

###### "Help! I tried updating my git submodules recursively but the submodule folder(s) are empty!"
When a new submodule is added to the parent repo and you would like to pull it onto your machine, you must first initialize the submodule before you are able to update it.
```
git pull
git submodule init
git submodule update --recursive
```

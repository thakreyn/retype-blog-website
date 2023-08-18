
### Tag
Every image on docker hub, has a tag associated to it. It is similar to a version for the image. By default, when we run an image using `docker run <image>` , it uses the `latest` tag by default.

To specify the tag, so that we can run an specific version, we can use `:` with the image name.

<center>Eg: docker run redis:4.0</center>

To know about all the versions and tags for an image, we can go to [Docker Hub](https://hub.docker.com/) and look for all the tags or versions in the description of an image.

### Detached Mode (-d)
To run the container in detached mode, we the `-d` flag. In case we have detached from a container and want to go back to it, we use the `attach` command.

### Name Container (--name)
It is used to give the container a unique name so that it can be easily referenced.
eg: `docker run --name <Name> <image>`

### STDIN
When we normally run a docker container, we are attached to the container session but we cannot pass any input to the container. We are just spectators and can read the outputs. 
To be able to provide our inputs to the container, we need to map our STDIN (standard input) to the containers. This is done with the `-i` flag. 

Eg: `docker run -i <image>` : Allows us to run in **interactive mode**. Which allows us to map our terminal STDIN to the container's.

If we want to completely map our terminal to the container, we use `-it` flag which represents **interactive terminal** mode.
Eg: `docker run -it <image>`

To exit from the interactive mode, type `exit` in the shell.

### Port Mapping (-p)
This is one of the most important concepts for the usability of docker. For a container to be useful, it needs to interact with the outside world. This is where port mapping comes in.
It allows us to map a port from our host to the container. 

> Host = Machine on which docker is running
>
> Container = Docker container in which our app is running

There are 2 ways to access this container: 
1. Internal IP : Every docker container is given an internal IP address. Thus we can use this IP address to access the container. But this allows only the 'Host' to access the container (as it is an internal IP). Any machine/user outside the host machine cannot use the internal IP.
2. Port mapping: For the container to be connected to the world, we need to map a port from the container (p1) to a port on the host (p2). This way all the traffic on the p2 is redirected to p1. To do this, we use the `-p` flag : `docker run -p <p2>:<p1> <image>`
   
   Eg: `docker run -p 8000:5000 ubuntu`  -> All the traffic from the host's port 8000 is forwarded to the port 5000 of the container. So when an external user hits port 8000 of host, it is actually hitting port 5000 of the container.

We can also map multiple ports from our container. Say we want to map port 5000 and 5500 from our container to 8000 and 8500 on host, we will call the `-p` flag twice.
Eg: `docker run -p 8000:5000 -p 8500:5500 <image>`

![](/resources/docker-run-1.png)
In the above, we can see that when we run the `ps` command, the ports are displayed. It shows that the port 8000 from the host (0.0.0.0) is mapped to 5000 of the container

> The internal IP of the docker container can be found in the networks section in `docker inspect`

### Volume Mapping (-v)
When a container is created, it has its own isolated filesystem. Any changes to the files within the container are its own. If we stop the container, the data is still there. But when we stop a container, all data is lost. If we want to avoid this, so that the data is still persisted even after we are done with container, we can map it to a local directory on the host using the `-v` flag.

`docker run -v <path on host>:<path on container> <image>`

> Observation: When using flags like `-v` and `-p`, the host always comes first and then the container. ie: `<host>:<container>`

### Environment Variables (-e)
Sometimes, we need to pass in environment variables to pass with the image. This can be done using the `-e` flag. This flag followed by the values sets the env variables for the image.
```shell
# Example
docker run -e POSTGRES_PASSWORD="secret" postgres
```

In case we need to view the environment variables for the container that is already running, we can use `docker inspect` to do that.

### Entrypoint (--entrypoint)
Whenever we need to override or provide a custom endpoint, we use this flag.
```shell
# Example
docker run --entrypoint sleep 10 ubuntu 
```

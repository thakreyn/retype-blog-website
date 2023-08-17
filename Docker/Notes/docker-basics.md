## What is a container?

It is a containerisation platform. The main difference between containers and VMs is that the VMs work on top of hardware level. They directly take that resource whereas the containers work on top of the base OS. They use the resources of the base OS to process.

> Unlike VMs, containers are not meant to run OS, they are meant to run only specific tasks. Once that task/process is done, the container is supposed to shut down.
>
> A CONTAINER LIVES ONLY AS LONG AS THE SERVICE INSIDE IT IS ALIVE
>
> If the service/process crashes, the container stops.
>
> Some use cases can be : Do a calculation, host service, host website etc.
>
> For eg: If we run a container without a service, it will start and immediately stop as there's no process.

## Basic Docker Commands:

### `run` - Start a container

`docker run <image-name>`
This starts a container with the given image name. It first checks for the image in the local host, if not found then goes to docker hub to get the image.
Each container that is created using this command is given a unique ID and name.

`docker run <image-name> <commands>`
Since a container requires a process that will be run on that, we can pass in a command while starting the container. This command will be run on the container as soon as it is started.

eg: `docker run ubuntu sleep 10` -> This will create a container using Ubuntu image and then run `sleep 10`. Thus the system will sleep for 10 seconds and after that, since there's no process left, the container will terminate.

> When we run a docker container using `docker run`, it waits for the container to finish and holds the terminal input till that time. We can view at the stdout of the container but can't give input.

The `run` command has a lot of flags and options.
[!ref](docker-run.md) : contains detailed information about the run command.

### `attach` - Attach to a running container

`docker attach <container-id>`
We can use this to attach to a running container. This way, we can look at its logs to the stdout. The container id doesn't need to be exact, only the first few unique characters are sufficient for that.

### `ps` - List Containers

`docker ps <options>`
It is used to list all the containers. By default it is used to list running containers. But `-a` can be used to view all types. Running, stopped etc.

### `stop` - Stop Container

`docker stop <container name / container ID>`
It is used to just stop a container from working. Doing this does not delete the container, it just pauses it, it still lives on the disk drive. The resources of the container are not cleared. If we want to do that, we use `rm`

We can pass multiple containers to the stop command.
eg: `docker stop aee1 qwe2 asd4 ...` and it will stop all of them at once.

### `rm` - Remove/Delete Container

`docker rm <container name / container ID>`
It completely deletes a container and its resources. (To delete a container, it first needs to be stopped)
Similar to `stop`, we can pass multiple containers to the `rm` command to remove all at once.

### `images` - List images

Shows all the images available on the local host. In case we need to download an image but not start a container from it. We use `docker pull <image name>`

`docker rmi <image-name>`

> We need to make sure that the image is not being actively used.

### `exec` - Run Commands on running container

`docker exec <commands>`
This is used to run commands on running docker containers.
eg: `docker exec echo /cat/etc`

### `inspect` - Inspect Container Details

Although `docker ps` gives us some information about the container, if we need additional comprehensive information, we use inspect.

`docker inspect <container name / ID>` shows the details about the container in a JSON format.

### `logs - View Container Logs`

When we run a container in detached mode, we cannot see the logs in our terminal directly. For this we use the logs command.

`docker logs <container name / ID>` shows all the STDOUT logs of the container.

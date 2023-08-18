
How do we create our own image? 
We might need to create our own image when we need to add something to an existing image or we want to create something fresh tailored to our need. 

We can imagine creating an image as parallel to deploying the code manually. Assuming we need to deploy a flask server, we will take the following steps:
1. Setup OS
2. update libraries
3. Install dependencies
4. Install python
5. pip install flask
6. Copy/Fetch the code
7. Run the server with the needed config

For creating an image, we follow a similar approach: 
All the steps that we need to perform to prepare the image are written to a [Docker File](/Docker/Notes/docker-file.md). And this is then used to build the container.

### Docker build

It is used to create an image from the dockerfile. 

`docker build <name of dockerfile> -t <image/tagname>` 

This creates a local image of the dockerfile. To push this image to docker hub, we use `docker push <image name>`

> Docker build command works on a directory and not single dockerfile.
> `-t` : used to give a name/tag to to the image

Eg: Creating an image with a tag : `docker build . -t name:tag`

### Layered Architecture

Each line in the [Docker File](/Docker/Notes/docker-file.md) represents a layer in the process of building the image. Each layer contains only the changes that are needed to come from the previous layer to current state. 
This allows it to be very efficient and fast.
Docker also stores the progress of each layer in cache. So that in case there is an error or changes to a layer, it picks up the previous layers from the cache and continues building on top of it.

### Commands v/s Entrypoint

Commands (CMD) are the commands that will run when the container has started. We can pass the values to CMD in 2 ways. String and JSON Array.

```dockerfile
CMD ["sleep", "5"]
OR
CMD sleep 5
```

This is a neat way of running a command instead of passing it every time in docker run.

But there might be cases, where might want to make it more dynamic, then in that case we use `ENTRYPOINT`

ENTRYPOINT is like CMD, but the only difference is that in case of CMD, any input afterwards or with the help of the command line override the CMD command whereas for ENTRYPOINT, it appends the additional commands.

```dockerfile
# 1. Only entrypoint
ENTRYPOINT ["sleep", "5"]

# 2. ENTRYPOINT WITH CMD (used to give default value. If value passed in run, then CMD is overriden)
ENTRYPOINT ["sleep"]
CMD ["5"]

# 3. ENTRYPOINT (completely waits for cli)
ENTRYPOINT ["sleep"]
```

If there is any case, where we need to pass in a different entrypoint altogether, we can do so by using `--entrypoint` with [Docker Run](/Docker/Notes/docker-run.md).


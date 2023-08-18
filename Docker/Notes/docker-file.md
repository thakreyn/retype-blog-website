---
title: DockerFile
---

A dockerfile is a text-file with the name `Dockerfile` that stores all the commands to build a docker image. It contains step by step instructions to build the image. 

Sample Dockerfile:
```dockerfile
FROM UBUNTU
RUN apt-get update
RUN apt-get install python

RUN pip install flask

COPY . /opt/code

ENTRYPOINT /opt/code flask run
```

The dockerfile follows a `INSTRUCTION ARGUMENT` format. Every line starts with an instruction (in all caps) followed by the argument for that instruction

* `FROM` : This instruction specifies the base image or OS for the image. Each dockerfile starts with a FROM instruction. 
  > Every dockerfile needs to have a base image or OS
* `RUN` : This instruction runs the arguments provided while building the image.
* `COPY` : Used to copy files
* `ENTRYPOINT` : Allows us to specify a command that will be run when the image is run.






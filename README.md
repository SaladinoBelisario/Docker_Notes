# **Docker notes**

## **Docker commands**

How to invoke a command:
> docker <command> <sub-command> (options)

To retrieve information about our docker configuration:
> docker info

To verify that docker can talk with CLI:
> docker version

### Images vs Containers
Images are the binaries containing all the libraries and resources.

Containers are the instances containing and image.

The docker default "registry" is called Docker Hub (hub.docker.com).

#### Running an example

> docker container run --publish 80:80 nginx

This command will expose an nginx instance in your machine (localhost).

¿What made the command?
* Downloaded the nginx image from Docker Hub
* Started a new container with that image
* Opened port 80 on the host IP
* Routes the traffic to the container IP, port 80

# **Docker notes**

## **Docker commands**

How to invoke a command:
> docker < command > < sub-command > (options)

To retrieve information about our docker configuration:
> docker info

To verify that docker can talk with CLI:
> docker version

### Images vs Containers
Images are the binaries containing all the libraries and resources and the 
metadata for running it.

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

Running in detached mode:
Docker can run containers in detached mode, in this way the container will 
run as a service independently:

> docker container run --publish 80:80 --detached nginx

Specifying the name for the container (needs to be unique):

> docker container run --publish 80:80 --detached --name [name] nginx

To delete our containers:

> docker container rm [id1] [id2] [id3]...

Docker in this scenario will:
* Look for the image in the local cache
* If not find anything will look in the repositories (Docker Hub in this case)
* Unless other thing specified docker will download the latest version
* Creates the container in the way previously discussed

### Container vs VMs

Containers aren't mini-vm's, containers are just processes and are limited only 
to the resources of the process.

Some useful commands for containers:

Process list in one container:
> docker container top

Details of one container config:
> docker container inspect

Performance stats for all containers:
> docker container stats

### Getting a Shell inside the container

> docker container run [command_to_run_in_container] --name [container_name]
> [container_image] [default_command]

## **Docker networking**

By default:
* Each container to a private network "bridge"
* Each virtual network routes trough NAT firewall on host IP
* All containers in a virtual network can talk with each others without -p
* Best practice is to create a new virtual network for each app.

Show networks:
> docker network ls

Inspect a network:
> docker network inspect

Create a network:
> docker network create --driver

Attach a network to container:
> docker network connect

Detach a network from container:
> docker network disconnect

### The DNS

DNS is the key for easy inter-container communications.

Forget about IP's, the Addresses can change in every deploy, **the solution 
is to use names**. To make this more easy you need to use your **custom networks**.

### DNS Round Robin

Next we're going to view an example of Networking with docker. Since we can have
multiple containers responding to the same DNS record we're going to see how 
to use the Round Robin technique.

Create a new network:
> docker network create [network_name]

Create 2 detached containers:
> docker container -d --net [network_name] --net-alias [network_alias] [image:version]

Make sure you have your instances in the same network:
> docker container ls

From another container(linux distribution) lookup for the alias created for the network:
> docker container run --rm --net [network_name] [another_container] nslookup [network_alias]

This will tell our linux distribution in the container to lookup for the instances that 
responses to such alias.

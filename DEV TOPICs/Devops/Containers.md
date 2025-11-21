## What are Containers?(it’s not a vm)

Containers are lightweight, standalone packages that contain everything needed to run a piece of software, including code, runtime, system tools, libraries, and settings. They provide consistency across different computing environments and make applications portable and efficient to deploy.

  

  

|cmds|purpose|
|---|---|
|docker ps|to check all the running imgaes|
|docker run -it --name docker-host --rm --privileged ubuntu:jammy|creating a docker img of ubuntu with name docker-host|
|docker exec -it docker-host bash|executing a predefined image|
|||

  

`docker run` means we're going to run some commands in the container, and the `-it` means we want to make the shell interactive (so we can use it like a normal terminal.)

cmd ==--rm== means don’t store any logs and files delete every thing ones and image is deleted .

cmd ==--privileged== is used to gain more access towared root directory

  

  

## chroot

basically what we do using this process is make feel a dir as root such that now it can’t access folders and file outside it thinking it’s the last stopage . (in docs how to do it ) we need to provide all the files to that folder to do it .

chroot doesn’t grantee security completely as even after assigning different root i can see all the processes going on on the computer, can kill processes, unmount filesystem and even hijack processes.

## namespaces

Namespaces allow you to hide processes from other processes. There's a lot more depth to namespaces beyond.(they even get different process PIDs, or process IDs, so they can't guess what the others have) The above is describing _just_ the PID namespace. There are more namespaces as well and this will help these containers stay isolated from each other.  
  

  

imagine a particular site takes up all the memory of entire system , taking down everyone else sites as well , or some one run dirty script for the same

## cgroup

preventing fork bomb is something crazy basically with this property we try to allocate resources to our user , like no of process , disk space , memory and various other

  

if you ever had create your own custom container use the doc mentioned below

[https://containers-v2.holt.courses/](https://containers-v2.holt.courses/)

  

---

## Docker in My words

it’s an engine to create and run a container.

### **What is Container?**

consider container as a separate environment for development which includes it’s own separate os , file system and various other aspects including all the dependencies our project required to execute it.

### **what is the benefit of containerizing a project ?**

containerizing a project helps to manage it easily such as dependencies are easily manageable it is easy to deploy the project and various other (include more points )

==Kubernetes/Container orchestration==

### **Difference between image and container ?**

container is the executed version of Image. Consider image as source code of a project containing everything to run it . But when we it’s build and executed its executed state is different similar analogy for container .

  

  

## Layers , Volume and Network

### Layers

Containers are build layer by layer with every command included in DockerFile.

Benefit of Layers : docker cache these layers and load them quickly if no change is made in them. reusable across different images, which makes building and sharing images more efficient.

This immutability is key to Docker's reliability and performance, as unchanged layers can be shared across images and containers.

  

[![](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fbc784ebf-de6a-47ac-bb73-5fd44ba82e46%2FScreenshot_2024-03-10_at_3.28.40_PM.png?table=block&id=f4d28211-1790-4f26-85da-9e55cf259f89&cache=v2)](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fbc784ebf-de6a-47ac-bb73-5fd44ba82e46%2FScreenshot_2024-03-10_at_3.28.40_PM.png?table=block&id=f4d28211-1790-4f26-85da-9e55cf259f89&cache=v2)

### Volume

you must have notices that as container is killed all the (newly) data stored inside the container gets deleted with it . Cause container are meant to be stateless. The state associated with is destroyed as soon as container is stopped.

Solution : so we come up with a thing called Volume consider it as a storage volume which is mounted with container so that all the state of container can be stored there.

### With volumes

1. Create a `volume`

```JavaScript
docker volume create volume_database
```

1. Mount the folder in `mongo` which actually stores the data to this volume

```JavaScript
docker run -v volume_database:/data/db -p 27017:27017 mongo
```

  
Docker makes the contents of the named volume `volume_database` available inside the container at the path `/data/db`. Any files the container writes to `/data/db` are saved in `volume_database` , persisting even if the container is deleted or recreated

1. Open it in MongoDB Compass and add some data to it

[![](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F8c867185-969d-4d72-b3be-d3cb386903ef%2FScreenshot_2024-03-10_at_3.44.35_PM.png?table=block&id=cdf21e0d-100b-4091-b832-3d4a3887d7ab&cache=v2)](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F8c867185-969d-4d72-b3be-d3cb386903ef%2FScreenshot_2024-03-10_at_3.44.35_PM.png?table=block&id=cdf21e0d-100b-4091-b832-3d4a3887d7ab&cache=v2)

[![](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fb8c8ef76-2b19-468f-8bed-2660f6c34496%2FScreenshot_2024-03-10_at_3.48.44_PM.png?table=block&id=62467d68-6603-418d-abcf-db442078e6ce&cache=v2)](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fb8c8ef76-2b19-468f-8bed-2660f6c34496%2FScreenshot_2024-03-10_at_3.48.44_PM.png?table=block&id=62467d68-6603-418d-abcf-db442078e6ce&cache=v2)

1. Kill the container

```JavaScript
docker kill <container_id>
```

1. Restart the container

```JavaScript
docker run -v volume_database:/data/db -p 27017:27017 mongo
```

1. Try to explore the database in Compass and check if the data has persisted (it will!)

  

### Network

What about communication between 2 different container.

for Network we need to understand that when we run a container it is mini machine itself containing several ports itself so while connecting any database using [localhost](http://localhost) won’t work as no dB is connected to the containers local port .

### Network Commands

1. Create a network

```JavaScript
docker network create my_custom_network
```

1. Start the `backend process` with the `network` attached to it

```JavaScript
docker run -d -p 3000:3000 --name backend --network my_custom_network image_tag
```

1. Start mongo on the same network

const mongoUrl: string = 'mongodb://mongo:27017/myDatabase';

here instead of [localhost](http://localhost) we used mongo (it is the name of container running mongodb)

```JavaScript
docker run -d -v volume_database:/data/db --name mongo --network my_custom_network -p 27017:27017 mongo(image name)
```

1. Check the logs to ensure the dB connection is successful

```JavaScript
docker logs <container_id>
```

  

There are 2 kind of networks mainly bridge and host , as the name suggest host network is the network of the host machine implies all the ports of host machine are accessible whereas in bridge network it consider mainly about bridge networks that are build to connect between the containers .

  

  

==One beautiful question asked in class : Can we mount two different container to same volume ?==

No we can’t the container didn’t run. Also there would be concurrency issue if do this . Also it doesn’t solve any issue.

## Docker-Compose

Basically it is a tool to run multiple docker container together. With bunch of commands described we can run multiple services with just one single command.

only practice will make you better here .

# Common docker commands

1. docker images
2. docker ps
3. docker run
4. docker build

### 1. docker images

Shows you all the images that you have on your machine

### 2. docker ps

Shows you all the containers you are running on your machine

### 3. docker run

Lets you start a container

1. p ⇒ let’s you create a port mapping
2. d. ⇒ Let’s you run it in detached mode

### 4. docker build

Lets you build an image. We will see this after we understand how to create your own `Dockerfile`

### 5. docker push

Lets you push your image to a registry

### 6. Extra commands

1. docker kill
2. docker exec
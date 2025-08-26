Docker Volumes and Bind Mounts|Persistent Storage for Docker| 
-------------------------------------------------------------


1. Problem Statement for Volumes (Why Volumes) ?
2. Bind Mounts
3. Volumes
4. Advantages of using Volumes over Bind Mounts
5. Lifecycle of Volumes
6. How to mount a Volume


Containers doesnot have file system , they are ephermeral.
Container use cpu, memeory etc fro the kernel of the host OS.
Thats whycontainer is light weight in nature.

Thus the log file detail fo teh containerwill also die when the container dies or is donw.


Problem 1
    - the container does nt have the previous logs of the nginx containers for securiy purpose or audits

Problem 2:
    - classic fornt end and backend  problme,because backend gone down or some other reason the problem occurs.

Problem 3.
    - your app is trying toe read some files from the host OS, but again it doesnot know how to rad that information.

to overcome this, docker came with 2 concpets:
    1.bind mounts
    2. volumes


// bind Mounts 

    - it will bind a folder on your container with a folder on the host
    ie any files that is present in the /app folder in the host, can be read by the container (vice versa)

    - your are binding the specific directory from teh host to the container.

// Volumes

    - offers a better lifecyccle
    - using Docker Cli, you can create volumes
    - logical partition on the host
    - create, destroy, attach volumes to diff containers
    - even using the volumes solves same issue for which bind is used.

    Advantages of volumes:
    - managing teh enitre thing using docker cli
    - volumes has a lifecycle.
    - you can create volume on any hosts, or external sources (backup) etc.
    - easy to share from one container to another.
    - high performance


// syntax
docker -v < source; destination; ------>

--mount srouce destination permission

--mount is better and more verbose option and the users can understand it.


// PRACTICAL
-   docker volume ls // lists the docker volumes

- docker volume create naufal   // when you do this there is a logical file system partition created in this specific file system

- docker volume inspect naufal  // get the details of this specific volume

-docker volume rm naufal // to delete the volume

- to mount a volume onto a container
    - docker images| head -5

    -dcoker build -t volumedemo dockerfile // create an image on the local
 
    - docker volume create naufal

    - docker run -d --mount source=naufal, target=/app, <image>

    - docker ps //list of running containers

    - docker inspect <container name>
        // here you can for the Mounts in this container

    // to delete teh volume
        - docker volume rm naufal
        some times this shows error, because it is might e already in use.
        so you need to firstly:
        - stop the container
        - delete

thus even if teh container goes, the data of that container is avaialable in teh /app directory f te volume called naufal, which can be shared to others.


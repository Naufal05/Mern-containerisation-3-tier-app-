Docker Networking | Bridge vs Host vs Overlay |Secure containers with custom bridge network
-----------------------------------------------------------------


In this video, you will learn about:
1. Why you need networking on Docker ?
2. How does Docker Networking Work ?
3. What are different types of Networking in Docker ?
4. Which Networking is default and Out of the Box ?
5. Play with Docker containers and inspect their Networks
6. Create a custom bridge network to secure it from other containers


0:00 - Introduction to Docker Networking
01:15 -Why Docker Networking with Example 
02:50 - Comparing Docker Networking with VM networking
05:40 - How containers talk to Host in terms of networking ?
07:15: Default Networking in Docker and What happens without that ?
09:30: Different types of Networking in Docker ?
10:40 - Host Networking in Docker
12:46 - Networking Deep Dive 
20:30 - Live Demo with Practical Examples


3 Tier Application
1. Frontend
2. Backend
3. Database


// create a network
- docker network create mern
-docker run --name=frontend --network=mern -d -p 5173:5173 mern-frontend

- docker logs frontend

Why is network required ?
- If we use a common network they can communicate easily.

Your host and container has different networks. You can connect a bridge between the host and container. But what if you had multiple containers, but it would be easy if we had common network in multiple services.


2, Run Database before the bckend

3. run backend

4. Docker Compose
i =s used to run multiple containers, making sure that the container dependency is taken care and if required a common networking can also be provided for the containers. This is how docker compose works.


docker ps

docker rm -f <container-id>


create docker-compose.yaml

in the yaml:

SERVICES // Define different container confg., how to start the container, where is the docker file, what is the port binding
	FRONTEND

	BACKEND
	  - depends on db
	DB

NETWORK


VOLUME



-------------------------------------------------------------

DOCKER NETWORKING
------------------
A VM is basically an operating system plus your application. 
So each of the application are completely isolated from each other.
Bydefault  you have the flexibility in your VM.

container doesnot have a complete operating system.

whereas Docker has 2 scenarios:
 1. one container should talk to another container (like frontend to backend)
 2. Logical Isolation of the containers example payment related container

So container networking offers solutions for both of them above.


bydefault each zero network is created for any container, VM etc in the host.

Suppose a container has a different subnet and host has different subnet then:
Docker creates a virtual network, without this virtual network your container cannot talk to your host.
	1. Bridge network (bydefault)	- Veth - Docker 0

	2. Host networking
		-host and container has a common network
		- insecure
	3.Overlay networking
		- popular 
		- useful in container orchestration.

Networking isolation is important for more secure (for example Finance container).

Create custom Bridge network
	- in docker, we can create custom bridge network.

// Practical example

-ls
-docker run -d --name login nginx:latest
-docker exec -it login /bin/bash	// logged in to login container
- apt update
- apt-get install iputils-ping -y 	// install ping
-docker run -d --name logout nginx:latest
-docker inspect login //to check the ip address
-docker inspect logout

from the login we ca ping to logout container, because they are using default bridge networking

- to list out all the networks on this host:
	-docker network ls

- to delete 
	- docker network rm <network name>

// now i want to create a finance container logically isolated from login container

	- docker network create secure-network

// assign the secure network to the finance container
	- docker run -d --name finance --network=secure-network nginx:latest

- docker inspect finance
- ping <ipaddress from the inspect>	//you wont be able to ping to secured network

//now run a container with the host network
	- docker run -d --name host-demo --network=host nginx:latest




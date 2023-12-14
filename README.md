# Deploy Web application Using Docker Swarm
The project aims to deploy a web application using Docker Swarm, an efficient container orchestration tool that simplifies the management and scaling of containerized applications.

## Requirements
1. Python 3.9
2. Node.js
3. React
4. Docker
5. AWS Account 

## Deploy process
#### Step 1. Create 3 ec2 instances called
* Swarm-manager
* Swarm-worker-1
* Swarm-worker-2


#### Step 2. Clone the repository to the ec2 
```
git clone https://github.com/NirmalNaveen20/swarm-web-deployment
```

#### Step 3. Install docker with docker engine in all 3 nodes
[Docker installation doumentaion](https://docs.docker.com/engine/install/ubuntu/)

Check docker version

`sudo docker --version`

#### Step 4. Run docker build to create image

```
sudo docker build -t notes-app
```

#### Step 5. Add inbound rules to all 3 nodes 
Custom TCP - 2377 - Anywhere IPV4
Custom TCP - 8000 - Anywhere IPV4

#### Step 6. Creating manager and worker nodes

Now open the "Swarm manager" node and run the command
```
sudo docker warm init
```

This will be creating empty swarm.

After initializing the swarm on the “swarm-manager” node, there will be one key generated to add other nodes into the swarm as a worker. Copy and run that key on the rest of the servers.

(Now, both machines are added to the swarm as a worker).

To check about all the nodes in the manager, run 
`sudo docker node ls`

#### Step 7. Push Docker image to the docker hub
Before push image to the docker hub, please login with your docker-hub credentials

`docker login`

Now tag the image using your docker-hub username and version

`sudo docker tag notes-app <docker-hub-username>/django-notes-app:latest`

Now push to the docker hub

`sudo docker push <docker-hub-username>/django-notes-app:latest`

#### Step 8. Create docker swarm service
Now Create a service in the docker manager node server to install the application through the docker image from the docker hub.

```
sudo docker service create --name django-notes-app --replicas 3 --publish 8000:8000 <docker-hub-username>/django-notes-app:latest
```

Now, run `sudo docker service ls` in manager mode you can see service is up and running

Now, this service will create a container in the Manager node and worker 01 and worker 02 node.

To check, run the command: `sudo docker ps`

Now, service will be running on all three nodes. To check this, just grab the Ip Address of any of the nodes followed by port 8000.

http://<public ip>:8000 check on all 3 nodes(manager, worker 1,2 [ec2 instances])

#### Step 9. Scale up and Scale down Docker swarm service containers

In Docker Swarm, you can scale up or scale down the number of replicas (tasks) for a service using the docker service scale command. This command allows you to change the number of replicas, effectively increasing or decreasing the desired number of instances for a particular service.

Scale Up: To increase the number of replicas for a service:

`docker service scale <service_name> = <replica_count>`

Make Drain(Inactive) to manager node using following command.

`docker node update --availability drain <node name>`


## Author

- [@Nirmal Naveen](https://www.nirmalnaveen.com/)

![Logo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTlPjhPV6D68kBoBq82reUr6ndqcI_n9YPSQ9WA3sqT_RAXpDVcujzTO1MmWrcmcGYeyA&usqp=CAU)


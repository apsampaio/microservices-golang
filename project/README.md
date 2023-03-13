## ✨ Using Docker Swarm

1. Build your microservice image using docker:
   `docker build -f logger-service.dockerfile -t apsampaio/logger-service:1.0.0`
2. Push the image to a hub, example dockerhub:
   `docker push apsampaio/logger-service:1.0.0`
3. Configure **swarm/yml** file
4. Init swarm with `docker swarm init`
5. Join the swarm node with `docker swarm join-token worker`
6. Run the swarm `docker stack deploy -c swarm.yml myapp`

### 🎊 Scaling Service

1. Run `docker service ls` to view the services list
2. Run `docker service scale myapp_listener-service=3`
3. Now you should have 3 instances of the service running
4. If you want to increase or decrease the number of instances, just run the same command with the desired number at the end

### 🧵 Updating Service

1. Build the dockerfile from the updated service:
   `docker build -f logger-service.dockerfile -t apsampaio/logger-service:1.0.1`
2. Push the update to the hub:
   `docker push apsampaio/logger-service:1.0.1`
   > A good tip is to duplicate the survice before updating, docker updates one instance at time, so you don't have down time in you application. This scaling can be done using the `docker service scale myapp_service=2`
3. Now you simply update the image:
   `docker service update --image apsampaio/logger-service:1.0.1`

### ❌ Disabling a Service or Stopping the Swarm

1. Run `docker service scale myapp_listener-service=0`. This will disable the listener service.
2. In case you want to stop the entire swarm, run `docker stack rm myapp`
3. To leave the swarm run `docker swarm leave --force`

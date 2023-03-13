## Using Docker Swarm

1. Build your microservice image using docker
   `docker build -f logger-service.dockerfile -t apsampaio/logger-service1.0.0`
2. Push the image to a hub, example dockerhub:
   `docker push tsawler/logger-service:1.0.0`
3. Configure **swarm/yml** file
4. Init swarm with `docker swarm init`
5. Run the swarm `docker stack deploy -c swarm.yml myapp`

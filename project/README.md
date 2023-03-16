## Docker Swarm

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

## Kubernetes with Minikube

### 🐧 Minikube Installation WSL2

1. Download the latest version of Minikube:
   `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
2. Make the binary executable:
   `chmod +x ./minikube`
3. Move the binary to your executable path:
   `sudo mv ./minikube /usr/local/bin/`

### 🧊 Kubectl Installation WSL2

1. Download the latest version of Kubectl:
   `curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/linux/amd64/kubectl`
2. Make the binary executable:
   `chmod +x ./kubectl`
3. Move the binary to your executable path:
   `sudo mv ./kubectl /usr/local/bin/kubectl`
4. Create the config file
   `mkdir -p ~/.kube`
   `ln -sf "/mnt/c/Users/<user>/.kube/config" ~/.kube/config`

### ✍ Minikube commands

- Starting with docker driver:
  `minikube start --driver=docker`
- Verify if the node was created:
  `kubectl get nodes`
- View running pods:
  `kubectl get pods -A`
- Open kubernetes dashboard:
  `minikube dashboard`
- Deploy a specific service with `kubectl apply -f k8s/rabbit.yml` or a complete folder with `kubectl apply -f k8s`
- View services running:
  `kubectl get svc`
- View services deployments:
  `kubectl get deployments`

### ⚖ LoadBalancer

- If the service is running, remove it with:
  `kubectl delete svc <service-name>`
- Run:
  `kubectl expose deployment <service-name> --type=LoadBalancer --port=<port> --target-port=<port>`
- Create the tunnel:
  `minikube tunnel`

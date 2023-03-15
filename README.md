# Working with Microservices in Go

![Go version](https://img.shields.io/badge/goversion-v1.20.x-blue)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=flat&logo=postgresql&color=555555)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&color=555555)
![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat&logo=docker&color=555555)
![RabbitMQ](https://img.shields.io/badge/-RabbitMQ-FF6600?style=flat&logo=rabbitmq&color=555555)
![Portainer](https://img.shields.io/badge/-Portainer-FF6600?style=flat&logo=portainer&color=555555)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-FF6600?style=flat&logo=kubernetes&color=555555)

<h1 align="center">
    <img src=".github/build.png" alt="Build" width="200px" />
</h1>

> This repo is my source code from the ![Udemy](https://img.shields.io/badge/-Working%20with%20Microservices%20in%20Go-white?style=flat&logo=udemy&color=A435F0&logoColor=white) course by [Trevor Sawler](https://github.com/tsawler)

## 🧩 Services

- broker-service: an optional single entry point to connect to all services from one place (accepts JSON;
  sends JSON, makes calls via gRPC, and pushes to RabbitMQ)
- authentication-service: authenticates users against a Postgres database (accepts JSON)
- logger-service: logs important events to a MongoDB database (accepts RPC, gRPC, and JSON)
- queue-listener-service: consumes messages from amqp (RabbitMQ) and initiates actions based on payload (sends via RPC)
- mail-service: sends email (accepts JSON)

All services (except the broker) register their access urls with etcd, and renew their leases automatically.
This allows us to implement a simple service discovery system, where all service URLs are accessible with
"service maps" in the Config type used to share application configuration in the broker service.

In addition to the microservices, the included `docker-compose.yml` at the root level of the project
starts the following services:

- Postgresql - used by the authentication service to store user accounts
- MongoDB - used by the logger service to save logs from all services
- etcd - used for service discovery
- mailhog - used as a fake mail server to work with the mail service

## 📚 Course content

1. [x] Introduction
2. [x] Building a simple front end and Microservice
3. [x] Building an Authentication Service
4. [x] Building a Logger Service
5. [x] Building a Mail Service
6. [ ] Building a Listener service: AMQP with RabbitMQ
7. [ ] Communicating between services using Remote Procedure Calls (RPC)
8. [ ] Speeding things up(potentialy) with gRPC
9. [ ] Deploying our Distributed App using Docker Swarm
10. [ ] Deploying our Distributed App to Kubernetes
11. [ ] Testing Microservices
12. [ ] Final Thoughts

## ☕ Using the project

From the root level of the project, execute this command (this assumes that you have
[GNU make](https://www.gnu.org/software/make/) and a recent version
of [Docker](https://www.docker.com/products/docker-desktop) installed on your machine):

```
make up_build
```

If the code has not changed, subsequent runs can just be `make up`.

Then start the front end:

```
make start
```

Hit the front end with your web browser at `http://localhost:80`. You can also access a web
front end to the logger service by going to `http://localhost:8082` (or whatever port you
specify in the `docker-compose.yml file`).

To stop everything:

```
make stop
make down
```

While working on code, you can rebuild just the service you are working on by
executing

`make auth`

Where `auth` is one of the services:

- auth
- broker
- logger
- listener
- mail

All make commands:

```
tcs@Grendel go-microservices % make help
 Choose a command:
  up               starts all containers in the background without forcing build
  down             stop docker compose
  build_auth       builds the authentication binary as a linux executable
  build_logger     builds the logger binary as a linux executable
  build_broker     builds the broker binary as a linux executable
  build_listener   builds the listener binary as a linux executable
  build_mail       builds the mail binary as a linux executable
  up_build         stops docker-compose (if running), builds all projects and starts docker compose
  auth             stops authentication-service, removes docker image, builds service, and starts it
  broker           stops broker-service, removes docker image, builds service, and starts it
  logger           stops logger-service, removes docker image, builds service, and starts it
  mail             stops mail-service, removes docker image, builds service, and starts it
  listener         stops listener-service, removes docker image, builds service, and starts it
  start            starts the front end
  stop             stop the front end
  test             runs all tests
  clean            runs go clean and deletes binaries
  help             displays help
```

---

Feito com ❤ by [Andre Sampaio](https://github.com/apsampaio) :wave:

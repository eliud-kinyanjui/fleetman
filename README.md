# Fleetman - Kubernetes Microservices Project - GitOps by ArgoCD

## Project Overview

Fleetman is a microservices-based vehicle tracking and fleet management application deployed on Kubernetes. The application allows for real-time tracking of vehicle positions, monitoring of vehicle telemetry, and management of fleet resources.

![Fleetman Application Screenshot](/assets/images/argocd.png)


## Architecture

The application consists of several microservices:

- **Webapp**: Frontend interface built with Angular, served by Nginx
- **API Gateway**: RESTful API service that coordinates requests between frontend and backend services
- **Position Simulator**: Generates vehicle position updates
- **Queue**: ActiveMQ message broker for asynchronous communication between services

## Deployment

The application is deployed on Kubernetes with the following resources:

```
NAME                                      READY   STATUS    RESTARTS   AGE
pod/api-gateway-6f6577d4b9-7jx2l          1/1     Running   0          38m
pod/position-simulator-68f7db9d7f-hl9jl   1/1     Running   0          24m
pod/position-tracker-6cd8b98649-6f8rd     1/1     Running   0          25m
pod/queue-6cb8f9669f-q55m4                1/1     Running   0          38m
pod/webapp-76f7f44667-nfb4r               1/1     Running   0          23m

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                          AGE
service/fleetman-api-gateway        NodePort    10.152.183.111   <none>        8080:30020/TCP                   38m
service/fleetman-position-tracker   ClusterIP   10.152.183.27    <none>        8080/TCP                         16m
service/fleetman-queue              NodePort    10.152.183.148   <none>        8161:30010/TCP,61616:31868/TCP   38m
service/fleetman-webapp             NodePort    10.152.183.237   <none>        80:30080/TCP                     38m


NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/api-gateway          1/1     1            1           38m
deployment.apps/position-simulator   1/1     1            1           38m
deployment.apps/position-tracker     1/1     1            1           25m
deployment.apps/queue                1/1     1            1           38m
deployment.apps/webapp               1/1     1            1           38m

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/api-gateway-6f6577d4b9          1         1         1       38m
replicaset.apps/position-simulator-68f7db9d7f   1         1         1       24m
replicaset.apps/position-tracker-6cd8b98649     1         1         1       25m
replicaset.apps/queue-6cb8f9669f                1         1         1       38m
replicaset.apps/webapp-76f7f44667               1         1         1       23m

```

## Setup Instructions

### Prerequisites

- Kubernetes cluster (minikube, kind, or cloud provider)
- kubectl CLI installed and configured

### Deployment Steps - using kubectl

1. Clone the repository:
   ```bash
   git clone https://github.com/eliud-kinyanjui/fleetman.git
   cd fleetman
   ```

2. Deploy the message queue:
   ```bash
   kubectl apply -f queue.yaml
   ```

3. Deploy the position simulator:
   ```bash
   kubectl apply -f position-simulator.yaml
   ```

4. Deploy the position tracker:
   ```bash
   kubectl apply -f position-tracker.yaml
   ```

5. Deploy the API gateway:
   ```bash
   kubectl apply -f api-gateway.yaml
   ```

6. Deploy the webapp:
   ```bash
   kubectl apply -f webapp.yaml
   ```
7. Deploy the services:
   ```bash
   kubectl apply -f service.yaml
   ```


## Accessing the Application

The application exposes several NodePort services:

- Web Interface: http://[node-ip]:30080
- API Gateway: http://[node-ip]:30020
- ActiveMQ Admin Console: http://[node-ip]:30010

Replace `[node-ip]` with your Node's IP address. If using minikube, you can get this with `minikube ip`.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
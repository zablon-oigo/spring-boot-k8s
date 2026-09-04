![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen?logo=springboot&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.34%2B-326CE5?logo=kubernetes&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-Ingress-009639?logo=nginx&logoColor=white)

## Spring Boot Deployment with Kubernetes and NGINX Ingress

This project demonstrates how to containerize a Spring Boot REST API and deploy it to Kubernetes using Docker, kind, and NGINX Ingress.

The application exposes a simple REST endpoint and runs with multiple replicas behind a Kubernetes Service. NGINX Ingress provides HTTP routing to the application. 


#### Architecture Diagram
<img width="989" height="466" alt="spring excalidraw" src="https://github.com/user-attachments/assets/b5586718-61b5-456e-9728-b8d5967cd085" />


#### Build the Spring Boot Application

Compile the application:
```sh
mvn clean compile
```
Package the application:
```sh
mvn clean package -DskipTests
```

#### Build the Docker Image
Build the Docker image:
```sh
docker build -t spring-demo .
```
Tag the image for Docker Hub:
```sh
docker tag spring-demo zablondev/spring-k8s-demo:1.0
```
Push the image:
```sh
docker push zablondev/spring-k8s-demo:1.0
```

#### Create the Kubernetes Cluster
Create the kind cluster using the provided configuration:
```sh
kind create cluster --config kind-config.yaml --name test
```
Verify the cluster:
```sh
kubectl get nodes
```
#### Deploy the Spring Boot Application
Apply the Deployment:
```sh
kubectl apply -f deployment.yaml
```
Check the Deployment:
```sh
kubectl get deployments
```
Check the Pods:
```sh
kubectl get pods
```
#### Create the Kubernetes Service
Apply the Service:
```sh
kubectl apply -f service.yaml
```
Check the Service:
```sh
kubectl get svc
```

#### Install NGINX Ingress Controller

Install the NGINX Ingress Controller for kind:
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.13.2/deploy/static/provider/kind/deploy.yaml
```

Verify that the NGINX Ingress Controller is running:
```sh
kubectl get pods -n ingress-nginx
```
#### Configure the Ingress

Apply the Ingress resource:

```sh
kubectl apply -f ingress.yaml
```
Check the Ingress:
```sh
kubectl get ingress
```
#### Configure Local DNS

Add the following entry to /etc/hosts:
```sh
127.0.0.1 spring.local
```
For example:
```sh
sudo nano /etc/hosts
```
Then add:
```sh
127.0.0.1 spring.local
```
#### Test the REST API
Once the application and NGINX Ingress Controller are running, test the endpoint:
```sh
curl http://spring.local/hello
```

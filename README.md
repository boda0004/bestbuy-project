# Project Assignment #2: Cloud-Native App Best-Buy Application

This repository contains the documentation, assests and deployment files for the Best Buy demo cloud-native application. The application follows a microservices architecture and incorporates AI capabilities for product descriptions and image generation.

## Application Architecture

The application has the following services: 

| Service            | Description                                                                          | GitHub Repo                                                                            | Notes                                 |
| ------------------ | ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | ------------------------------------- |
| `store-front`      | Web app for customers to place orders (Vue.js)                                       | [bestbuy-store-front](https://github.com/boda0004/bestbuy-store-front)                 | –                                     |
| `store-admin`      | Web app used by store employees to view orders in queue and manage products (Vue.js) | [bestbuy-store-admin](https://github.com/boda0004/bestbuy-store-admin)                 | –                                     |
| `order-service`    | This service is used for placing orders (JavaScript)                                 | [bestbuy-order-service](https://github.com/boda0004/bestbuy-order-service)             | Uses Azure service bus for messaging. |
| `product-service`  | This service is used to perform CRUD operations on products (Rust)                   | [bestbuy-product-service](https://github.com/boda0004/bestbuy-product-service)         | –                                     |
| `makeline-service` | This service handles processing orders from the queue and completing them (Golang)   | [bestbuy-makeline-service](https://github.com/boda0004/bestbuy-makeline-service)       | –                                     |
| `ai-service`       | Optional service for adding generative text and graphics creation (Python)           | [bestbuy-ai-service](https://github.com/boda0004/bestbuy-ai-service)                   | Uses GPT‑4 and DALL‑E‑3 models.       |
| `rabbitmq`         | RabbitMQ for an order queue                                                          | [bestbuy-rabbitmq](https://github.com/boda0004/bestbuy-rabbitmq)                       | –                                     |
| `mongodb`          | MongoDB instance for persisted data                                                  | [bestbuy-mongo](https://github.com/boda0004/bestbuy-mongo)                             | –                                     |
| `virtual-customer` | Simulates order creation on a scheduled basis (Rust)                                 | [bestbuy-virtual-customer-L8](https://github.com/boda0004/bestbuy-virtual-customer-L8) | –                                     |
| `virtual-worker`   | Simulates order completion on a scheduled basis (Rust)                               | [bestbuy-virtual-worker-L8](https://github.com/boda0004/bestbuy-virtual-worker-L8)     | –                                     |

### Diagram
[BestBuy architecture diagram](<Bestbuy architechture diagram.jpg>)

### Architecture Overview

- **Store‑Front**: Vue.js microservice that powers the customer-facing storefront, allowing users to browse products and place orders.  
- **Store‑Admin**: Vue.js administrative portal where staff can manage inventory and monitor incoming orders.  
- **Order‑Service**: Node.js service that accepts order requests and publishes them to RabbitMQ for downstream processing.  
- **Product‑Service**: Rust‑based microservice responsible for CRUD operations on the product catalog, interacting with MongoDB and the AI‑Service.  
- **Makeline‑Service**: Go service that consumes order messages from RabbitMQ, processes them, and marks orders as complete.  
- **AI‑Service**: Python service integrating OpenAI’s GPT‑4 and DALL‑E 3 APIs to generate dynamic product descriptions and imagery.  
- **RabbitMQ**: Message broker that decouples services by handling the order message queue.  
- **MongoDB**: NoSQL database used to persist product details and order records.  


## Prerequisites

- **Azure subscription** to create and manage your AKS cluster.  
- **Docker Engine** installed locally for building and pushing container images.  
- **kubectl** CLI configured to connect to your AKS cluster.  
- A **RabbitMQ** broker (e.g., deployed via Helm in AKS) for handling order messages.  
- A **MongoDB** instance to store product and order data.  
- **OpenAI API** credentials for GPT‑4 and DALL‑E‑3 access.  


## Steps

1. **Clone the infrastructure repo**  
   ```bash
   git clone https://github.com/boda0004/bestbuy-project
   cd bestbuy-project

2. **Build & Push Docker Images**
Run these for each service directory (store‑front, store‑admin, order‑service, product‑service, makeline‑service, ai‑service):

    cd <service-name>
    docker build -t boda0004/<service-name>:latest .
    docker push boda0004/<service-name>:latest
    cd ..

3. **Deploy Microservices to AKS**
kubectl create namespace bestbuy

    kubectl apply -f k8s/store-front.yaml      -n bestbuy
    kubectl apply -f k8s/store-admin.yaml      -n bestbuy
    kubectl apply -f k8s/order-service.yaml    -n bestbuy
    kubectl apply -f k8s/product-service.yaml  -n bestbuy
    kubectl apply -f k8s/makeline-service.yaml -n bestbuy
    kubectl apply -f k8s/ai-service.yaml       -n bestbuy

4. **Verify Deployment**
    kubectl get pods,svc -n bestbuy


## Run the app on Azure Kubernetes Service (AKS)

You can use the kubernetes YAML files provided in the [Deployment Files](./Deployment%20Files/) folder to deploy the app to an AKS cluster.




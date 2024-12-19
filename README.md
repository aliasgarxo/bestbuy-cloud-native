# BestBuy Cloud-Native Application

## Project Title
Building a Cloud-Native Application for Best Buy and Hosting it on Azure Kubernetes Service

---

## Overview
This project demonstrates a scalable and cloud-native architecture for Best Buy’s online platform, featuring microservices for customer and employee functionalities, AI-powered product enhancements, and robust deployment on Azure Kubernetes Service (AKS). Each microservice is containerized and hosted independently in its repository, ensuring modular development and deployment. The application adheres to 12-factor app principles, offering high availability, scalability, and maintainability.

---

## Application Services

### Services and Descriptions
| Service               | Description                                                                |
|-----------------------|----------------------------------------------------------------------------|
| **BestBuy-App**       | Customer-facing app for browsing and placing orders.                      |
| **BestBuy-Employee**  | Employee-facing app for managing products and viewing orders.             |
| **Order-Service**     | Handles order creation and sends data to the managed order queue.         |
| **Product-Service**   | Handles CRUD operations for product data.                                 |
| **Makeline-Service**  | Processes orders by reading from the order queue.                         |
| **AI-Service**        | Generates product descriptions and images using GPT-4 and DALL-E models. |
| **Database**          | MongoDB for persisting order and product data.                            |

---

## Architecture Explanation

The architecture follows a microservices-based design to ensure modularity and scalability. Key components include:

1. **API Gateway**: Central access point that routes client requests to the appropriate microservice.
2. **BestBuy-App**: Interfaces with the Order-Service and Product-Service to allow customers to browse products and place orders.
3. **BestBuy-Employee**: Integrates with the Product-Service and Makeline-Service, enabling employees to manage products and monitor orders.
4. **Order-Service**: Connects to a managed message queue (e.g., Azure Service Bus) or RabbitMQ to handle order processing.
5. **Product-Service**: Communicates with the MongoDB database to handle product data CRUD operations.
6. **Makeline-Service**: Reads orders from the queue and updates order statuses in MongoDB.
7. **AI-Service**: Leverages Azure OpenAI Services to generate product descriptions and images, enhancing the customer experience.

![Blank diagram (12)](https://github.com/user-attachments/assets/df42fb47-19d2-46a5-987b-90bdf5792096)

---

## Deployment Instructions

### Prerequisites
1. Azure account with permissions to create resources.
2. Azure CLI and Kubernetes CLI (kubectl) installed locally.
3. Docker installed locally for building images.

### Steps

#### 1. Create an AKS Cluster
```bash
az aks create --resource-group <resource-group-name> --name <aks-cluster-name> --node-count 1 --generate-ssh-keys
az aks get-credentials --resource-group <resource-group-name> --name <aks-cluster-name>
```

#### 2. Deploy Azure OpenAI Service
- Create an Azure OpenAI resource.
- Deploy GPT-4 and DALL-E models.
- Obtain API keys and update the deployment YAML files with the credentials.

#### 3. Set Up Azure Service Bus (Optional)
- Create a Service Bus namespace and queue.
- Update the environment variables in the deployment file with Service Bus details.

#### 4. Deploy the Application
```bash
git clone https://github.com/aliasgarxo/bestbuy-cloud-native.git
cd bestbuy-cloud-native/deployment-files
kubectl apply -f bestbuy-app-deployment.yaml
```
Verify the deployment:
```bash
kubectl get pods
kubectl get svc
```

#### 5. Access the Application
- Use the external IP addresses of the **BestBuy-App** and **BestBuy-Employee** services to access the portals.

---

## Microservice Integration

### How the Services Work Together
- **BestBuy-App** interacts with **Order-Service** for order placement and **Product-Service** for product details.
- **BestBuy-Employee** interacts with **Makeline-Service** for order processing and **Product-Service** for inventory management.
- **Order-Service** queues order requests in Azure Service Bus or RabbitMQ.
- **Makeline-Service** retrieves orders from the queue and updates their status in MongoDB.
- **AI-Service** provides product descriptions and images, consumed by the **Product-Service**.

---

## Tables

### Microservice Repositories
| Service               | Repository Link                                    |
|-----------------------|---------------------------------------------------|
| **BestBuy-App**       | [BestBuy-App Repo](https://github.com/aliasgarxo/bestbuy-app)          |
| **BestBuy-Employee**  | [BestBuy-Employee Repo](https://github.com/aliasgarxo/bestbuy-employee)|
| **Order-Service**     | [Order-Service Repo](https://github.com/aliasgarxo/order-service-bb)   |
| **Product-Service**   | [Product-Service Repo](https://github.com/aliasgarxo/product-service-bb) |
| **Makeline-Service**  | [Makeline-Service Repo](https://github.com/aliasgarxo/makeline-service-bb) |
| **AI-Service**        | [AI-Service Repo](https://github.com/aliasgarxo/ai-service-bb)         |

### Docker Images
| Service               | Docker Image Link                                  |
|-----------------------|---------------------------------------------------|
| **BestBuy-App**       | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/bestbuy-app/general) |
| **BestBuy-Employee**  | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/bestbuy-employee/general) |
| **Order-Service**     | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/order-service-bb/general) |
| **Product-Service**   | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/product-service-bb/general) |
| **Makeline-Service**  | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/makeline-service-bb/general) |
| **AI-Service**        | [Docker Image](https://hub.docker.com/repository/docker/dellhoak/ai-service/general)        |


---


## Bonus: CI/CD Pipeline
- CI/CD pipelines are implemented for all microservices.
- Workflows include building, testing, and deploying services.
- Check the **Actions** tab or `ci_cd.yaml` in each repository for pipeline details.

---

## Key Features
- Modular design with independent microservices.
- Kubernetes deployment ensuring high availability.
- AI-powered enhancements for product descriptions and images.
- Scalable and maintainable architecture for Best Buy’s online platform.

---

For further details or issues, feel free to raise an issue in the respective repositories.


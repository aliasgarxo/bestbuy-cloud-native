# Lab Project Assignment #2: Building a Cloud-Native App for Best Buy

## Scenario
As a Full-Stack Cloud-Native Developer at Best Buy, I have been tasked with developing a demo cloud-native application for Best Buy's online store. The application must follow the architecture of the Algonquin Pet Store (On Steroids) with a managed backing service (e.g., Azure Service Bus) replacing RabbitMQ for the Order Queue Service.

## Assignment Objectives
1. **Implement a cloud-native application** using microservices architecture.
2. **Develop and deploy** a full-stack solution using Kubernetes.
3. **Enable AI-powered product descriptions and image generation** using GPT-4 and DALL-E.
4. **Demonstrate functionality** by deploying the app in a Kubernetes cluster.

---

## Application Overview

### Services
| Service         | Description                                                                |
|------------------|----------------------------------------------------------------------------|
| **BestBuy-App**  | Customer-facing app for browsing and placing orders.                      | 
| **BestBuy-Employee**  | Employee-facing app for managing products and viewing orders.             | 
| **Order-Service-bb**| Handles order creation and sends data to the managed order queue.         | 
| **Product-Service-bb** | Handles CRUD operations for product data.                              | 
| **Makeline-Service-bb**| Processes orders by reading from the order queue.                      | 
| **AI-Service-bb**   | Generates product descriptions and images using GPT-4 and DALL-E models. | 
| **Database**     | MongoDB for persisting order and product data.                            | 

---

### Architecture Diagram

![Blank diagram (12)](https://github.com/user-attachments/assets/df42fb47-19d2-46a5-987b-90bdf5792096)


---

## Deployment Instructions

### Prerequisites
1. Azure account with permissions to create resources.
2. Azure CLI and Kubernetes CLI (kubectl) installed locally.
3. Docker installed locally for building images.

### Steps to Deploy the Application
1. **Create an AKS Cluster**:
   - Use Azure CLI to create an AKS cluster.
   - Connect to the cluster using `kubectl`.

2. **Deploy Azure OpenAI Service**:
   - Create Azure OpenAI resource.
   - Deploy the GPT-4 and DALL-E models.
   - Obtain the credentials (API keys and endpoints) and update the `bestbuy-app-deployment.yaml` file.

3. **Set Up Azure Service Bus (Optional)**:
   - Create a Service Bus namespace and queue.
   - Update the environment variables in the deployment file with Service Bus details.

4. **Deploy the Application**:
   - Clone the GitHub repository containing all microservices and deployment files.
     ```bash
     git clone https://github.com/aliasgarxo/CST8915-Assignment_2.git
     ```
   - Navigate to the `Deployment Files` folder.
   - Run the following command:
     ```bash
     kubectl apply -f bestbuy-app-deployment.yaml
     ```
   - Verify the deployment:
     ```bash
     kubectl get pods
     kubectl get svc
     ```

5. **Access the Application**:
   - Use the service endpoints (retrieved via `kubectl get svc`) to access the **Best Buy App** and **Best Buy Employee Portal**.

---

## Tables

### Microservice Repositories
| Service          | Repository Link                                    |
|-------------------|---------------------------------------------------|
| **BestBuy-App**   | [BestBuy-App Repo](https://github.com/aliasgarxo/BestBuy-App)                 |
| **BestBuy-Employee**   | [BestBuy-Employee Repo](https://github.com/aliasgarxo/BestBuy-Employee)                 |
| **Order-Service** | [Order-Service Repo](https://github.com/aliasgarxo/order-service-bb)               |
| **Product-Service** | [Product-Service Repo](https://github.com/aliasgarxo/product-service-bb)           |
| **Makeline-Service** | [Makeline-Service Repo](https://github.com/aliasgarxo/makeline-service-bb)         |
| **AI-Service**    | [AI-Service Repo](https://github.com/aliasgarxo/ai-service-bb)|

### Docker Images
| Service          | Docker Image Link                                 |
|-------------------|--------------------------------------------------|
| **BestBuy-App**   | [BestBuy-App Docker Image](https://hub.docker.com/repository/docker/dellhoak/bestbuy-app/general)    |
| **BestBuy-Employee**   | [BestBuy-Employee Docker Image](https://hub.docker.com/repository/docker/dellhoak/bestbuy-employee/general)    |
| **Order-Service** | [Order-Service Docker Image](https://hub.docker.com/repository/docker/dellhoak/order-service-bb/general)  |
| **Product-Service** | [Product-Service Docker Image](https://hub.docker.com/repository/docker/dellhoak/product-service-bb/general) |
| **Makeline-Service** | [Makeline-Service Docker Image](https://hub.docker.com/repository/docker/dellhoak/makeline-bb/general) |
| **AI-Service**    | [AI-Service Docker Image](https://hub.docker.com/repository/docker/dellhoak/ai-service/general)     |

---

## Demo Video
Check out the application in action by watching the demo video: [YouTube Link](https://youtu.be/swdxzTQ-MiA).

---

## Issues and Limitations

### Makeline-Service
- **Issue**: I faced difficulties configuring the `Makeline-Service` to work with Azure Service Bus. As a result, the active orders are not reflected on the **Employee Portal**.
- **Resolution**: I temporarily replaced Azure Service Bus with RabbitMQ and deployed RabbitMQ as a StatefulSet in the Kubernetes cluster to address the issue.

---

## Bonus Task: CI/CD Pipeline
I have implemented a CI/CD pipeline for each microservice. You can check the **Actions** tab in the GitHub repository for each service or review the `ci_cd.yaml` workflow file in the respective repository.

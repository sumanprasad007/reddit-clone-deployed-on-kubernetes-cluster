# Deploying Reddit Clone Web App on Kubernetes Cluster: A Step-by-Step Guide

## Introduction

Container orchestration with Kubernetes has become a standard for deploying and managing applications efficiently. In this detailed guide, we will walk you through the process of deploying a Reddit clone web application onto a Kubernetes cluster hosted on AWS. By following these steps, you will not only learn the deployment process but also gain insights into the power of Kubernetes for managing containerized applications.

## Use Cases of This Project 🌐

This Reddit clone web application deployed on a Kubernetes cluster has several potential use cases with minimal modifications:

- Multi-Tenant Web Hosting: Host multiple instances of the Reddit clone for different users or organizations within the same Kubernetes cluster.

- Load Balancing: Easily scale your Reddit clone horizontally by adjusting the number of replicas in the Deployment, ensuring high availability and improved performance.

- Microservices Architecture: Extend this project by breaking down your Reddit clone into microservices, each running in its own container, and manage them efficiently with Kubernetes.

- API Gateway: Integrate an API gateway to manage and secure access to different services within your application, making it more robust and secure.


## Step 1: Provision AWS Instances 🌐

Begin by accessing the AWS console and creating two instances with specific configurations:

**CI-Server:**
- AMI: Ubuntu
- Type: t2.micro

**Deployment Server:**
- AMI: Ubuntu
- Type: t2.medium

Ensure that you name these instances accordingly for easy reference.

## Step 2: Clone the Code on CI-Server 📂

On the CI-Server, clone the codebase of your Reddit clone web application.

## Step 3: Prerequisites 🛠️

**On CI-Server:**
- Update the server: `sudo apt-get update`
- Install Docker: `sudo apt-get install docker.io -y`
- Give permission to Docker: `sudo usermod -aG docker $USER && newgrp docker`

**On Deployment Server:**
- Update the server: `sudo apt-get update`
- Install Docker: `sudo apt-get install docker.io -y`
- Give permission to Docker: `sudo usermod -aG docker $USER && newgrp docker`
- Install Minikube and Kubectl:
    ```bash
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    sudo snap install kubectl — classic
    minikube start — driver=docker
    ```

## Step 4: Write a Dockerfile for the Project 🐳

Create a Dockerfile for your Reddit clone project with the following content:

```Dockerfile
FROM node:19-alpine3.15

WORKDIR /reddit-clone

COPY . /reddit-clone

RUN npm install

EXPOSE 3000

CMD ["npm","run","dev"]
```

## Step 5: Build and Push Docker Image 🚀

Build the Docker image from the Dockerfile using the following command:

```
docker build . -t chxtan/reddit-clone

```

After building the image, push it to Docker Hub by running:


```
docker login
# Enter your username and password
docker push chxtan/reddit-clone

```

## Step 6: Connect to Deployment Server 🖥️

SSH into the Deployment Server.

## Step 7: Install Minikube and Kubectl

Ensure Minikube and Kubectl are installed on the Deployment Server.

## Step 8: Deploy Docker Image 🚢

Now, it's time to deploy the Docker image from Docker Hub onto your Kubernetes cluster.

## Step 9: Create a Kubernetes Folder 📁
Create a folder named "K8s" on the Deployment Server.

## Step 10: Create a Deployment YAML File 📝
Inside the "K8s" folder, create a file named "Deployment.yml" and add the following content:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reddit-clone-deployment
  labels:
    app: reddit-clone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: reddit-clone
  template:
    metadata:
      labels:
        app: reddit-clone
    spec:
      containers:
        - name: reddit-clone
          image: trainwithshubham/reddit-clone
          ports:
            - containerPort: 3000

```

## Step 11: Apply the Deployment File 📜
Apply the Deployment file to create pods:

```
kubectl apply -f Deployment.yml

```


## Step 12: Verify Deployments 🧐
To ensure that the deployments are successful, use the following command:

```
kubectl get deployments

```

## Step 13: Create a Kubernetes Service 🌐
Now, you need to create a Service to provide an IP address for your application within the cluster.


## Step 14: Create a Service YAML File 📝
Inside the "K8s" folder, create a file named "Service.yml" and add the following content:

```
apiVersion: v1
kind: Service
metadata:
  name: reddit-clone-service
  labels:
    app: reddit-clone
spec:
  type: NodePort
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 31000
  selector:
    app: reddit-clone

```

## Step 15: Apply the Service File 📜
Apply the Service file to create a service:

```
kubectl apply -f Service.yml

```

## Step 16: Check the Service 🌐
To verify that the service is created successfully, run:

```
kubectl get services

```

## Step 17: Access the Application 🚀
You can now access your application by visiting the URL provided by Minikube:

```
minikube service reddit-clone-service — url

```

## Step 18: Expose the Application to the Internet 🌍
To make your application accessible from the internet, you'll need to configure port forwarding.

## Step 19: Port Forwarding 🔄
Run the following command for port forwarding:

```
kubectl port-forward svc/reddit-clone-service 3000:3000 –address 0.0.0.0

```

## Step 20: Access Your Live Reddit Clone 🌐

Visit your Reddit Clone web app at the provided domain or IP address, and it should now be accessible to the world.


# Conclusion

Congratulations! 🎉 You've successfully deployed a Reddit clone web app on a Kubernetes cluster hosted on AWS. This guide has covered the entire process, from provisioning AWS instances to advanced routing using Ingress. Kubernetes offers powerful capabilities for container orchestration, making it an essential tool for modern application deployment and management. Explore the various use cases to adapt this project to your specific needs.

```

This README.md file provides a comprehensive step-by-step guide for deploying a Reddit Clone Web App on a Kubernetes cluster hosted on AWS, including the additional steps you requested.

```

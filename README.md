# snap-stack-app

A simple image upload application built with **Python** and **Streamlit**, containerized using **Docker**, and deployed on a **Kubernetes** cluster with **Azure File Storage** for persistent storage.

---

## Table of Contents

- [Overview](#overview)  
- [Features](#features)  
- [Architecture](#architecture)  
- [Setup and Deployment](#setup-and-deployment)  
  - [Docker Image](#docker-image)  
  - [Azure Container Registry (ACR)](#azure-container-registry-acr)  
  - [Kubernetes Deployment](#kubernetes-deployment)  
    - [Static PVC Provisioning](#static-pvc-provisioning)  
    - [Dynamic PVC Provisioning](#dynamic-pvc-provisioning)  
- [Verification](#verification)  
- [Tech Stack](#tech-stack) 

---

## Overview

`snap-stack-app` is a simple Streamlit-based image upload web application developed in Python. It is containerized with Docker, stored in Azure Container Registry (ACR), and deployed to a Kubernetes cluster with persistent storage backed by Azure File Storage.

Both **static** and **dynamic** Persistent Volume Claim (PVC) provisioning approaches are implemented for storing uploaded images in Azure File Storage.

---

## Features

- Intuitive Streamlit UI for uploading images  
- Containerized with Docker for easy deployment  
- Kubernetes deployment with Azure File Storage-backed persistent volumes  
- Supports both static and dynamic PVC provisioning methods  
- Uploaded images persistently stored on Azure File Storage  

---

## Architecture

User
↓
Streamlit App (snap-stack-app)
↓
Docker Container → Kubernetes Pod
↓
Persistent Volume Claim (PVC)
↓
Azure File Storage (Azure Storage Account)

yaml
Copy
Edit

---

## Setup and Deployment

### Docker Image

1. Build the Docker image:

   ```bash
   docker build -t snap-stack-app:v1 .
2. Tag the image for your Azure Container Registry (ACR):
   ```bash
   docker tag snap-stack-app:latest <your-acr-name>.azurecr.io/snap-stack-app:V1
3. Push the image to ACR:
    ```bash
    docker push <your-acr-name>.azurecr.io/snap-stack-app:v1

### Azure Container Registry (ACR)

- The Docker image is stored in Azure Container Registry.

- Ensure your Kubernetes cluster has permission to pull images from your ACR.

- This can be done via:

  - Azure AD integration with your AKS cluster, or

  - Creating Kubernetes image pull secrets with ACR credentials and linking them to your service account.

---

### Kubernetes Deployment

#### Static PVC Provisioning

- Create an Azure Storage Account and Azure File Share.

- Create a Kubernetes Persistent Volume (PV) pointing to the Azure File Share.

- Create a Kubernetes Secret containing Azure Storage Account credentials.

- Create a Persistent Volume Claim (PVC) that binds to the PV.

- Deploy your application Pod mounting the PVC to persist uploaded images.

#### Dynamic PVC Provisioning

- Use an Azure File Storage StorageClass configured in your cluster.

- Create a PVC manifest requesting storage from this StorageClass.

- Deploy the Pod manifest mounting the PVC.

- Kubernetes dynamically provisions and binds the Persistent Volume for you.

---

## Verification

- Access the Streamlit web UI via the Kubernetes service.

- Upload images using the interface.

- Confirm successful upload messages.

- Check Azure File Storage (via Azure Portal or CLI) to ensure images are stored.

- Tested and verified for both static and dynamic PVC setups.

---

## Tech Stack

- Python 3.x

- Streamlit

- Docker

- Kubernetes

- Azure Container Registry (ACR)

- Azure Kubernetes Service (AKS) or Kubernetes Cluster

- Azure File Storage

- YAML for Kubernetes manifests

# Distribusion DevOps Challenge

Welcome to the **distribusion DevOps Challenge** repository! This document provides an overview of the challenge, its requirements, and the solution implementation.

---

## Table of Contents

- [Distribusion DevOps Challenge](#distribusion-devops-challenge)
  - [Table of Contents](#table-of-contents)
  - [Challenge Overview](#challenge-overview)
    - [**Core Requirements**](#core-requirements)
  - [Solution Architecture](#solution-architecture)
    - [**Prerequisites**](#prerequisites)
    - [**Commands**](#commands)
  - [Infrastructure as Code (Terraform)](#infrastructure-as-code-terraform)
  - [Kubernetes Deployment (Helm)](#kubernetes-deployment-helm)
    - [**Features**](#features)
    - [**Deploying the Application**](#deploying-the-application)
      - [Deploy metalLB](#deploy-metallb)
      - [Deploy Ingress-Nginx](#deploy-ingress-nginx)
      - [Deploy Database](#deploy-database)
      - [Deploy Vikunja ToDo List application](#deploy-vikunja-todo-list-application)
  - [Monitoring](#monitoring)
    - [**Implemented Tools**](#implemented-tools)
  - [Installtion and Configuration: Monitoring Tools](#installtion-and-configuration-monitoring-tools)
  - [IAM with keycloak](#iam-with-keycloak)

---

## Challenge Overview

Distribusion is on the lookout for talented **DevOps Engineers** to help maintain and deploy applications efficiently. As part of this challenge, candidates must design and implement a **containerized**, **scalable**, and **automated** solution for deploying a **Vijunka TODO List App** application.

### **Core Requirements**

✔️ Use any k8s platform **(kind,eks,gke)**
✔️ Deploy it to a **Kubernetes cluster**
✔️ Use **Terraform** to provision the infrastructure **(Use kind here)**
✔️ Ensure **monitoring** is in place **(Ingress-Nginx Service Metrics issue on mac)**

---

## Solution Architecture

The solution follows a modular and cloud-agnostic approach with the following components:

- **Application:** Vikonji TODO List app.
- **Infrastructure:** Use here local tool (kind k8s)
- **Deployment:** Kubernetes manifests managed via **Kustomize**
- **Monitoring:** Integrations with **Prometheus, Grafana**

---


### **Prerequisites**

- Kind Cluster configured

### **Commands**

```sh
cd infra/kind/qa/cluster.yml
kind create cluster --config cluster.yml
```

---

## Infrastructure as Code (Terraform)

In general Terraform is used to provision the **EKS cluster** and associated resources in AWS. But for this challenge i used kind cluster.


## Kubernetes Deployment (Helm)

A **Kustomize** is used to deploy the Vikonji TODO List application to Kubernetes.

### **Features**

✔️ Structured **kustomize config** for modularity
✔️ Configurable **overlays** for easy customization
✔️ Supports **rolling updates** and **scalability**

### **Deploying the Application**

#### Deploy metalLB
```sh
cd k8s/mainfest/metal-lb
kubectl apply -f metallb-native.yaml
kubectl apply -f metal-lb.yml
```
#### Deploy Ingress-Nginx
```sh
cd k8s/mainfest/ingress-nginx
kustomize build --enable-alpha-plugins overlays/dev | kubectl apply -f -
```

#### Deploy Database
[**Why use local database resource for this challenge**](k8s/mainfest/vikunja/components/README.md)

  Here database service(psql) is deployed together with application.
  I used the kustomize components feature to add database as part of application
  deployment

  [Kusromize component](k8s/mainfest/vikunja/components/database/README.md)


#### Deploy Vikunja ToDo List application
```sh
cd k8s/mainfest/vikunja
kustomize build --enable-alpha-plugins overlays/dev | kubectl apply -f -
```

---

## Monitoring

The system includes monitoring tools to ensure availability and performance.

### **Implemented Tools**

✔️ **Prometheus & Grafana** for real-time monitoring


  Installtion and Configuration: [Monitoring Tools](k8s/mainfest/monitoring/README.md)
---

## IAM with keycloak

***Spend some time to configure and implement keycloak for SSO configuration, but since i am using kind, the ingress endpoint is not publically accessable which prevent me to configure the correct callback url, since all urls are working only with port-forward method.***

***In general i worked with configure SSO using azure ad application where all those endpoint are work correctly using the dex***

# Azure DevOps CI/CD Pipeline – Java Web App (Linux App Service)

## Overview

This repository contains a production-style, multi-stage Azure DevOps YAML pipeline that builds, analyzes, packages, and deploys a Maven-based Java web application to Azure App Service (Linux).

The implementation reflects enterprise CI/CD design principles, including stage isolation, artifact promotion, secure authentication, and quality-gated deployments.

---

## Architecture

![CI/CD Architecture](docs/ci-cd-architecture.png)

### Workflow Summary

The pipeline is automatically triggered on commits to the **main** branch and executes the following stages:

### **1. CodeScan**
- Static code analysis using SonarQube  
- Unit test execution and result publishing  
- Quality gate enforcement before build progression  

### **2. Build & Package**
- Application compilation using Apache Maven  
- WAR/JAR artifact generation  
- Artifact staging and publishing for downstream deployment  

### **3. Deploy**
- Secure authentication via Azure Service Principal  
- Deployment to Azure App Service (Linux)  
- Target environment: **dev**  

---

## Design Principles

- **Multi-stage YAML architecture** for separation of concerns  
- **Artifact lifecycle management** between build and deployment  
- **Quality-first workflow** with enforced static analysis  
- **Secure cloud authentication** using Azure Resource Manager connection  
- **Deterministic builds** triggered from source control  

---

## Technologies Used

- Azure DevOps (YAML Pipelines)  
- SonarQube (Static Code Analysis)  
- Apache Maven (Build Automation)  
- Azure App Service (Linux)  
- Azure Resource Manager (Service Principal Authentication)  

---

## Engineering Focus

This project demonstrates practical CI/CD pipeline architecture, secure cloud deployment patterns, artifact traceability, and quality-controlled release workflows aligned with modern Azure cloud engineering standards.

---

**Role Focus:** Azure Cloud Engineering / DevOps

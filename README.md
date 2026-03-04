Azure DevOps CI/CD – Java Web App to Azure Linux App Service
Overview

This repository contains a production-style, multi-stage Azure DevOps pipeline that builds, scans, packages, and deploys a Maven-based Java web application to Azure App Service (Linux).

The pipeline is fully defined in YAML and structured to enforce quality gates, artifact traceability, and controlled cloud deployments.

Architecture

The workflow is intentionally separated into three stages:

CodeScan → Build → Deploy

CodeScan integrates SonarQube to enforce static analysis and publish unit test results.

Build compiles and packages the application using Apache Maven and publishes versioned artifacts.

Deploy releases the artifact to Azure App Service (Linux) using an Azure Resource Manager service connection.

The pipeline runs on a managed agent pool and is triggered on changes to the main branch.

Engineering Decisions

Multi-stage YAML pipeline for environment separation and scalability.

Artifact publishing between stages to decouple build and deployment.

Service Principal authentication to avoid credential exposure.

Quality-first workflow — code must pass analysis before deployment.

Explicit deployment targeting using environment configuration (dev).

This mirrors enterprise CI/CD design patterns rather than single-stage demo pipelines.

What This Demonstrates

Azure DevOps pipeline architecture

Static code analysis integration

Artifact lifecycle management

Secure Azure authentication

Linux-based App Service deployment

CI/CD maturity aligned with cloud engineering practices

Next Iteration (Production Hardening)

Introduce IaC (Terraform/Bicep) for App Service provisioning

Add gated approvals for higher environments

Implement blue/green or slot-based deployment strategy

Integrate Azure Monitor / Application Insights

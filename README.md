# End-to-End CI/CD Pipeline | Azure DevOps | Azure App Service (Linux)

A production-grade, multi-stage Azure DevOps YAML pipeline that builds, quality-gates, packages, and deploys a Maven-based Java web application to Azure App Service (Linux).

This project demonstrates enterprise CI/CD design: stage isolation, artifact promotion, secure least-privilege authentication, and quality-gated deployments where a failed SonarQube scan blocks production progression entirely.

---

## Pipeline Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   STAGE 1       │────▶│   STAGE 2        │────▶│   STAGE 3       │
│   CodeScan      │     │   Build          │     │   Deploy        │
│                 │     │                  │     │                 │
│ • SonarQube     │     │ • Maven compile  │     │ • ARM service   │
│   static        │     │ • Unit tests     │     │   connection    │
│   analysis      │     │ • WAR artifact   │     │ • Azure App     │
│ • Quality gate  │     │   staging        │     │   Service push  │
│   (hard block)  │     │ • Artifact       │     │ • Linux runtime │
│                 │     │   publish        │     │   deployment    │
└─────────────────┘     └──────────────────┘     └─────────────────┘
        │
        ▼
   FAILS HERE = no deploy
   Pipeline stops at quality gate
```

---

## Key Design Decisions

**Dynamic agent pool routing**
Pool selection is resolved at execution time through conditional expressions and variable group bindings — not hardcoded. This means the pipeline can route to different agent pools (self-hosted vs Microsoft-hosted) without modifying YAML.

**SonarQube as a hard blocking gate**
Static code analysis is not advisory — it is a mandatory pass/fail gate. A quality gate failure stops the pipeline at Stage 1. Nothing gets built or deployed if the code does not meet the defined quality threshold.

**Scoped ARM service connections**
Each environment uses a separately scoped Azure Resource Manager service connection with least-privilege permissions. The dev connection cannot deploy to staging or production. This enforces environment isolation at the authentication layer, not just at the pipeline logic layer.

**Artifact staging decouples build from deploy**
The build stage publishes artifacts to Azure Pipelines artifact storage before the deploy stage begins. This means the deploy stage consumes a fixed, immutable artifact — not a live build output. If deploy fails, you can re-run deploy without re-running the build.

---

## Technologies

| Tool | Purpose |
|------|---------|
| Azure DevOps | Pipeline orchestration and YAML pipeline engine |
| SonarQube | Static code analysis and quality gate enforcement |
| Apache Maven | Java build automation and dependency management |
| Azure App Service (Linux) | Target deployment environment |
| Azure Resource Manager | Scoped service principal authentication |

---

## Repository Structure

```
Projectotu/
├── azure-pipelines.yml          # Main pipeline (3-stage: CodeScan, Build, Deploy)
├── azure-pipelines--1.yml       # Pipeline variant 1
├── azure-pipelines--2.yml       # Pipeline variant 2
├── src/main/webapp/             # Java web application source
├── pom.xml                      # Maven project configuration
└── docs/                        # Architecture diagrams and documentation
```

---

## What This Demonstrates

- Multi-stage Azure DevOps YAML pipeline design
- Quality-gated CI/CD with SonarQube enforcement
- Least-privilege ARM service connections scoped per environment
- Artifact lifecycle management between pipeline stages
- Production-grade deployment patterns for Azure App Service (Linux)
- Dynamic agent pool selection via variable group bindings

---

## Author

**Mary Okpala** — DevOps Engineer | Azure | CI/CD | IaC  
[github.com/MaryOkpala](https://github.com/MaryOkpala)

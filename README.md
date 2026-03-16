# CI/CD Learning Roadmap for Software Development & AI Production

## Progress

| # | Topic | Status | Notes |
|---|-------|--------|-------|
| 1 | Version Control (Git) | Done | Already known before starting |
| 2 | Core CI/CD Concepts | Done | Docs: `docs/02-core-cicd-concepts.md` |
| 3 | CI/CD Platforms | In Progress | Built first GitHub Actions pipeline (`.github/workflows/ci.yml`) |
| 4 | Pipeline Design | - | |
| 5 | Automated Testing | - | |
| 6 | Code Quality | - | |
| 7 | Docker | - | |
| 8 | Kubernetes | - | |
| 9 | Infrastructure as Code | - | |
| 10 | Cloud Platforms | - | |
| 11 | Deployment Strategies | - | |
| 12 | Monitoring | - | |
| 13 | Post-Deployment | - | |
| 14 | MLOps Fundamentals | - | |
| 15 | ML Pipeline Orchestration | - | |
| 16 | Model Training in CI/CD | - | |
| 17 | Model Evaluation & Validation | - | |
| 18 | Model Serving & Deployment | - | |
| 19 | LLM-Specific CI/CD | - | |
| 20 | Data Pipeline CI/CD | - | |
| 21 | ML Monitoring in Production | - | |
| 22 | Security in CI/CD (DevSecOps) | - | |
| 23 | Best Practices | - | |

## What Was Built

- **Repo:** https://github.com/KienTranOMRE/CICD
- **Demo app:** `app/calculator.py` — simple Python calculator used as CI target
- **Tests:** `tests/test_calculator.py` — pytest suite with coverage
- **Pipeline:** `.github/workflows/ci.yml` — 5-stage GitHub Actions pipeline (lint → test → build → deploy staging → deploy production)
- **First pipeline run:** Failed at Stage 1 (Lint) due to import sort order in `tests/test_calculator.py` — demonstrates how CI catches issues before code reaches production

## Lessons Learned

1. **Pipeline as Code** works — the entire CI/CD pipeline is a single YAML file versioned alongside the source code
2. **Sequential stages enforce quality** — the pipeline failed at lint, so test/build/deploy never ran. Broken code cannot reach production.
3. **GitHub CLI (`gh`)** needs the `workflow` scope to push files under `.github/workflows/`
4. **First failure is a good thing** — it proves the pipeline is actually checking your code, not just rubber-stamping it

---

## Part 1: Foundations

### 1. Version Control -- DONE
- Git fundamentals (branching, merging, rebasing)
- Branching strategies (GitFlow, trunk-based development, GitHub Flow)
- Monorepo vs polyrepo
- Semantic versioning

### 2. Core CI/CD Concepts -- DONE
- What is Continuous Integration (CI)
- What is Continuous Delivery (CD)
- What is Continuous Deployment
- CI vs CD vs CD (delivery vs deployment)
- Build automation
- Pipeline as code
- Artifact management
- Environment promotion (dev -> staging -> production)

---

## Part 2: CI/CD Tools & Platforms

### 3. CI/CD Platforms
- GitHub Actions
- GitLab CI/CD
- Jenkins
- CircleCI
- Travis CI
- Azure DevOps Pipelines
- AWS CodePipeline / CodeBuild
- Argo CD (GitOps)

### 4. Pipeline Design
- Pipeline stages (build, test, deploy)
- Parallel and sequential jobs
- Pipeline triggers (push, PR, schedule, manual)
- Caching and artifact passing between stages
- Pipeline templates and reusable workflows
- Secrets and environment variable management
- Matrix builds

---

## Part 3: Testing in CI/CD

### 5. Automated Testing
- Unit testing
- Integration testing
- End-to-end (E2E) testing
- Smoke testing
- Regression testing
- Performance / load testing
- Security testing (SAST, DAST)
- Test coverage thresholds and quality gates

### 6. Code Quality
- Linting and formatting (ESLint, Prettier, Black, Ruff)
- Static analysis (SonarQube, CodeClimate)
- Dependency vulnerability scanning (Dependabot, Snyk)
- License compliance checks
- Code review automation

---

## Part 4: Containerization & Orchestration

### 7. Docker
- Dockerfile best practices
- Multi-stage builds
- Image optimization and layer caching
- Container registries (Docker Hub, ECR, GCR, GHCR)
- Docker Compose for local development

### 8. Kubernetes
- Kubernetes fundamentals (pods, services, deployments, ingress)
- Helm charts
- Kustomize
- Namespace and resource management
- ConfigMaps and Secrets
- Health checks (liveness, readiness probes)

---

## Part 5: Infrastructure & Deployment

### 9. Infrastructure as Code (IaC)
- Terraform
- AWS CloudFormation
- Pulumi
- Ansible for configuration management

### 10. Cloud Platforms
- AWS (EC2, ECS, EKS, Lambda, S3, CloudFront)
- GCP (GKE, Cloud Run, Cloud Functions)
- Azure (AKS, App Service, Functions)

### 11. Deployment Strategies
- Rolling deployment
- Blue-green deployment
- Canary deployment
- Feature flags (LaunchDarkly, Unleash)
- Rollback strategies
- Zero-downtime deployments
- A/B testing at infrastructure level

---

## Part 6: Monitoring & Observability

### 12. Monitoring
- Application Performance Monitoring (Datadog, New Relic)
- Log aggregation (ELK stack, Loki, CloudWatch)
- Metrics collection (Prometheus, Grafana)
- Distributed tracing (Jaeger, OpenTelemetry)
- Alerting and incident management (PagerDuty, Opsgenie)

### 13. Post-Deployment
- Health check endpoints
- Deployment verification testing
- Automated rollback on failure
- SLIs, SLOs, and SLAs

---

## Part 7: CI/CD for AI/ML Production

### 14. MLOps Fundamentals
- MLOps vs DevOps - what's different
- ML lifecycle (data, training, evaluation, deployment, monitoring)
- Experiment tracking (MLflow, Weights & Biases, Neptune)
- Model versioning and model registry
- Data versioning (DVC)
- Reproducibility of training runs

### 15. ML Pipeline Orchestration
- Kubeflow Pipelines
- Apache Airflow
- Prefect
- ZenML
- Metaflow
- Vertex AI Pipelines

### 16. Model Training in CI/CD
- Automated training pipelines
- GPU/TPU resource management in CI
- Hyperparameter tuning automation
- Training on cloud (SageMaker, Vertex AI, Azure ML)
- Distributed training considerations

### 17. Model Evaluation & Validation
- Automated model evaluation in pipelines
- Data drift detection
- Model performance regression tests
- A/B testing for models
- Shadow deployment (running new model alongside old)
- Champion/challenger patterns

### 18. Model Serving & Deployment
- Model serving frameworks (TorchServe, TF Serving, Triton, BentoML)
- REST API vs gRPC for model inference
- Batch inference vs real-time inference
- Model containerization
- Serverless model deployment (Lambda, Cloud Functions)
- Edge deployment considerations

### 19. LLM-Specific CI/CD
- LLM evaluation pipelines (evals, benchmarks)
- Prompt versioning and testing
- RAG pipeline CI/CD
- Fine-tuning pipelines
- LLM gateway and routing (LiteLLM, OpenRouter)
- Cost monitoring for LLM inference
- Guardrails and safety testing in CI

### 20. Data Pipeline CI/CD
- Data validation (Great Expectations, Pandera)
- Feature store management (Feast, Tecton)
- ETL/ELT pipeline testing
- Data quality checks in CI
- Schema evolution and migration

### 21. ML Monitoring in Production
- Model performance monitoring
- Data drift and concept drift detection
- Prediction logging and auditing
- Model retraining triggers
- Feedback loops

---

## Part 8: Security & Best Practices

### 22. Security in CI/CD (DevSecOps)
- Secret management (Vault, AWS Secrets Manager)
- Container image scanning (Trivy, Snyk)
- Supply chain security (SLSA, Sigstore)
- SBOM (Software Bill of Materials)
- RBAC for pipelines
- Signed commits and artifacts
- OIDC for cloud authentication in CI

### 23. Best Practices
- Trunk-based development with CI
- Small, frequent deployments
- Immutable infrastructure
- GitOps principles
- Documentation as code
- Disaster recovery and backup strategies
- Cost optimization for CI/CD infrastructure

---

## Suggested Learning Order

1. Git & version control
2. Docker basics
3. One CI/CD platform (GitHub Actions recommended)
4. Automated testing in pipelines
5. Deployment strategies
6. Kubernetes fundamentals
7. Infrastructure as Code (Terraform)
8. Monitoring & observability
9. Security practices
10. MLOps & ML pipelines
11. Model serving & deployment
12. LLM-specific CI/CD
13. Advanced topics (GitOps, canary deployments, chaos engineering)

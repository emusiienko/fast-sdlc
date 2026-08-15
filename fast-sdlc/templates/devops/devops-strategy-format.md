<!-- Default format for the DevOps Strategy artifact — accept as-is or customize with devops during Setup. Generated once per project. This is the single source of truth for every DevOps decision — nothing about cloud provider, IaC, CI/CD, or deployment lives outside it. -->

# DevOps Strategy

> **Status:** Draft — pending user approval
> **Date:** YYYY-MM-DD

## Project Profile

- **Setup type**: monolith / microservices / serverless / on-premise / hybrid [skip if defined in Architecture]
- **Deployment target**: containers/Docker | serverless functions | VMs | PaaS (e.g. Vercel/Railway/Heroku) | bare metal
- **Greenfield or brownfield**: new project from scratch / existing infrastructure to import into IaC
- **Compliance requirements**: GDPR / HIPAA / SOC 2 / PCI-DSS / none

## Cloud & Repository

- **Cloud provider**: AWS / GCP / Azure / on-premise / other — or undecided
- **Repository provider + account tier**: GitHub Free/Pro/Team/Enterprise | GitLab Free/Premium/Ultimate | other (tier affects available branch protection features and CI/CD minutes)
- **DNS / domain provider**: [needed to configure environment URLs — e.g. Route 53, Cloudflare, Google Domains]
- **Container registry** [if containerised]: ECR / GCR / GHCR / Docker Hub / other

## Environments

| Environment | Purpose | Branch | Deploy trigger |
|---|---|---|---|
| dev | Active development | develop | Manual via deploy.sh |
| staging | Pre-production validation | main (pre-merge) | Manual via deploy.sh |
| prod | Production | main | Manual via deploy.sh |

## Branch Strategy

- `main` (production — protected)
  - `develop` (integration — protected)
    - `feat/<slug>`
    - `fix/<slug>`
    - `chore/<slug>`

Branch protection rules:
- `main`: requires PR from develop, requires CI pass, no force push, no direct commits
- `develop`: requires PR from feature branches, requires CI pass

Note any account-tier limitations affecting branch protection here (see BRANCH PROTECTION in the agent instructions for how these rules get applied).

## IaC

- **Tool**: Terraform (cloud-agnostic default)
- **State backend**: [to be decided — remote state in S3/GCS/Azure Blob with locking]
- **Structure**: `infra/modules/` for reusable modules; `infra/environments/<env>/` for per-env configs
- **Variable files**: one `.tfvars` file per environment — no hardcoded values

## CI/CD Pipelines

### `.github/workflows/ci.yml`

Runs on every PR to `develop` and `main`. **No test steps** — testing is managed separately when a QA agent is introduced.

Steps:
- Lint (language-specific: ESLint/Prettier for JS/TS, Black/ruff for Python)
- Type check (tsc --noEmit for TS, mypy for Python)
- Build verification (confirm the project builds without errors)
- Security scan (dependency vulnerabilities — `npm audit`, `pip-audit`, `trivy`)

Must complete in under 5 minutes. Every failing step must output a clear, actionable error message.

### `.github/workflows/deploy.yml`

Does NOT trigger automatically. Provides a reusable workflow called by `deploy.sh`. Steps:
- Validate IaC (`terraform validate` / `terraform plan`)
- Build artefact
- Push to registry (if containerised)
- Apply IaC changes
- Post-deploy smoke check (HTTP health endpoint ping)

### `deploy.sh`

Single, documented deployment entry point. The user runs this — the DevOps agent never calls it autonomously.

```bash
#!/usr/bin/env bash
# Usage: ./deploy.sh <env> [--rollback]
# Environments: dev | staging | prod
# Requires: cloud credentials configured locally or via CI
```

## Secrets Management

- No secrets in code, `.env` files committed to the repo, or CI environment variables in plain text
- Secrets stored in: [cloud secret manager — AWS Secrets Manager / GCP Secret Manager / Azure Key Vault]
- Referenced from IaC as data sources — never hardcoded in `.tf` files
- `.env.example` lists required variables with placeholder values only

## Deployment

- Single entry point: `./deploy.sh <env>` (requires valid cloud credentials)
- The user runs deployments — the DevOps agent prepares and validates, never triggers
- Rollback: `./deploy.sh <env> --rollback` or equivalent IaC state rollback

## Rollback Procedure

1. Identify the last known-good state (git tag or Terraform state version)
2. Run `./deploy.sh <env> --rollback` to revert
3. Verify with post-deploy smoke check

## Infrastructure Ownership

| Area | Owner |
|---|---|
| IaC files | DevOps agent (via PRs) |
| Cloud console manual steps | User |
| deploy.sh execution | User |
| Secret rotation | User |

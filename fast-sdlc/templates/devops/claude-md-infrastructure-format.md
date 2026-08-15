<!-- Default format for the CLAUDE.md Infrastructure section — accept as-is or customize with devops during Setup. Added and maintained in CLAUDE.md after setup; devops owns this section only. -->

```markdown
## Infrastructure

| Environment | URL | Deploy |
|---|---|---|
| dev | https://dev.example.com | `./deploy.sh dev` |
| staging | https://staging.example.com | `./deploy.sh staging` |
| prod | https://example.com | `./deploy.sh prod` |

**IaC**: `infra/` (Terraform)
**Secrets**: [cloud secret manager name and region]
**Container registry**: [if applicable]
```

## CI/CD Pipeline

```
├── Automation & CI/CD
│ ├── GitHub Actions Workflows
│ │ ├── Static Code Analysis
│ │ │ ├── TFLint
│ │ │ ├── Checkov
│ │ │ └── Terraform format & validation
| | |
| | ├── Terraform Plan Validation
│ │ │ ├── Runs `terraform plan` for target modules
│ │ │ ├── Uses environment-specific tfvars (from branch/env_name)
│ │ │ └── Blocks unsafe changes
│ │ │
│ │ ├── Backend State Validation
│ │ │ ├── Detects target environment (dev / prod)
│ │ │ ├── Checks if remote Terraform backend exists
│ │ │ ├── Initializes Terraform backend
│ │ │ ├── Verifies Azure Storage state access
│ │ │ └── Ensures correct environment isolation
│ │ │
│ │ ├── Backend Bootstrap (Conditional)
│ │ │ ├── Triggered **only if backend does not exist**
| | | ├── Determines environment from branch (dev/staging/main)
│ │ │ ├── Creates Azure Resouce Group, Storage Account & Container
│ │ │ ├── Azure Authentication & Gives RBAC
│ │ │ ├── Creates Event Hub & Monitoring After RBAC
│ │ │ ├── Migrates local state to remote backend
│ │ │ └── Exposes `env_name` output for downstream workflows
│ │ │
│ │ ├── Terraform Deployment (Includes Destruction)
│ │ │ ├── Manual approval for production
│ │ │ ├── Applies infrastructure changes
│ │ │ └── Uses locked remote state
│ │ │
│ │ └── Website Content Deployment
│ │ ├── Trigger: Push to main branch (Static Site changes)
│ │ ├── Azure authentication (OIDC / Service Principal)
│ │ ├── Dynamic VMSS discovery (per Resource Group)
│ │ └── az vmss run-command invoke
│ │ └── Pulls latest static site content from GitHub
│ │
│ └── Update Script (update_site.sh)
│ ├── Git fetch & reset to latest commit
│ ├── Copies static site files
│ ├── Applies permissions
│ └── Restarts Apache
```

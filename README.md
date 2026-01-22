# Terraform Multi-Cloud Deploy - Reusable Workflow

![Built with Kiro.dev](https://img.shields.io/badge/Built_with-Kiro.dev-brightgreen?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEyIDJMMTMuMDkgOC4yNkwyMCA5TDEzLjA5IDE1Ljc0TDEyIDIyTDEwLjkxIDE1Ljc0TDQgOUwxMC45MSA4LjI2TDEyIDJaIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K)&nbsp;![Terraform](https://img.shields.io/badge/Terraform-7B42BC?logo=terraform&logoColor=white)&nbsp;![Release](https://github.com/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/actions/workflows/release.yaml/badge.svg)&nbsp;![Commit Activity](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![File Count](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf)&nbsp;![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/06e35985280456b113298ed56c626e73/raw/tf-deploy-multi-reusable-wf.json?)

A reusable GitHub Actions workflow for deploying Terraform infrastructure across multiple cloud providers (AWS, GCP, Azure, Snowflake, Databricks) with comprehensive validation, linting, and deployment capabilities.

## 🚀 Features

- **Multi-Cloud Support**: Deploy to AWS, Google Cloud Platform, Microsoft Azure, Snowflake, or Databricks
- **Comprehensive Pipeline**: Automated linting, validation, planning, and deployment
- **Flexible Backend**: Support for both S3 and HCP Terraform Cloud backends
- **Security First**: All sensitive credentials handled as secrets
- **Validation Pipeline**: TFLint → Terraform Validate → Terraform Plan → Terraform Apply
- **Reusable Design**: Can be called from any repository workflow

## 🏗️ Workflow Jobs

The workflow consists of four sequential jobs:

1. **🔧 TFLint** - Lints Terraform code for best practices and errors
2. **✅ Validate** - Validates Terraform configuration syntax
3. **🔍 Plan** - Creates and reviews Terraform execution plan
4. **🚀 Apply** - Applies the Terraform changes to infrastructure

## 📋 Inputs

### Required Inputs

| Input | Description | Type | Required |
|-------|-------------|------|----------|
| `cloud-provider` | Target cloud provider (`aws`, `gcp`, `azure`, `snowflake`, `databricks`) | string | ✅ |
| `tflint-ver` | TFLint version to install | string | ✅ |

### Optional Inputs

| Input | Description | Type | Default |
|-------|-------------|------|---------|
| `backend-type` | Backend type (`s3` or `remote` for HCP Terraform Cloud) | string | `s3` |
| `aws-region` | AWS region for authentication | string | - |
| `snowflake-account` | Snowflake account identifier | string | - |
| `snowflake-user` | Snowflake user name | string | - |
| `snowflake-role` | Snowflake role name | string | - |
| `tf-vars-file` | Terraform variables file path | string | `terraform.tfvars` |

## 🔐 Secrets

### HCP Terraform Cloud
| Secret | Description | Required When |
|--------|-------------|---------------|
| `tfc-token` | HCP Terraform Cloud API token | `backend-type` is `remote` |

### AWS Authentication
| Secret | Description | Required When |
|--------|-------------|---------------|
| `aws-role-to-assume` | AWS IAM role ARN to assume | `cloud-provider` is `aws` |

### GCP Authentication
| Secret | Description | Required When |
|--------|-------------|---------------|
| `gcp-wif-provider` | GCP Workload Identity Federation provider | `cloud-provider` is `gcp` |
| `gcp-service-account` | GCP service account email | `cloud-provider` is `gcp` |

### Azure Authentication
| Secret | Description | Required When |
|--------|-------------|---------------|
| `azure-client-id` | Azure client ID | `cloud-provider` is `azure` |
| `azure-tenant-id` | Azure tenant ID | `cloud-provider` is `azure` |
| `azure-subscription-id` | Azure subscription ID | `cloud-provider` is `azure` |

### Snowflake Authentication
| Secret | Description | Required When |
|--------|-------------|---------------|
| `snowflake-private-key` | Snowflake private key for authentication | `cloud-provider` is `snowflake` |

### Databricks Authentication
| Secret | Description | Required When |
|--------|-------------|---------------|
| `databricks-host` | Databricks workspace URL | `cloud-provider` is `databricks` |
| `databricks-token` | Databricks personal access token | `cloud-provider` is `databricks` |

## 📁 Expected Directory Structure

The workflow expects your Terraform files to be organized as follows:

```
your-repo/
├── infra/
│   ├── aws/
│   │   └── tf/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── terraform.tfvars
│   ├── gcp/
│   │   └── tf/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── terraform.tfvars
│   ├── azure/
│   │   └── tf/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── terraform.tfvars
│   ├── snowflake/
│   │   └── tf/
│   │       ├── main.tf
│   │       ├── variables.tf
│   │       └── terraform.tfvars
│   └── databricks/
│       └── tf/
│           ├── main.tf
│           ├── variables.tf
│           └── terraform.tfvars
└── .github/
    └── workflows/
        └── deploy.yml
```

## 📖 Usage Examples

### Basic AWS Deployment

```yaml
name: Deploy to AWS

on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/.github/workflows/terraform-deploy.yaml@main
    with:
      cloud-provider: aws
      tflint-ver: v0.52.0
      backend-type: s3
      aws-region: us-east-1
    secrets:
      aws-role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
```

### GCP Deployment with HCP Terraform Cloud

```yaml
name: Deploy to GCP

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/.github/workflows/terraform-deploy.yaml@main
    with:
      cloud-provider: gcp
      tflint-ver: v0.52.0
      backend-type: remote
      tf-vars-file: production.tfvars
    secrets:
      tfc-token: ${{ secrets.TF_CLOUD_TOKEN }}
      gcp-wif-provider: ${{ secrets.GCP_WIF_PROVIDER }}
      gcp-service-account: ${{ secrets.GCP_SERVICE_ACCOUNT }}
```

### Azure Deployment

```yaml
name: Deploy to Azure

on:
  pull_request:
    types: [closed]
    branches: [main]

jobs:
  deploy:
    if: github.event.pull_request.merged == true
    uses: subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/.github/workflows/terraform-deploy.yaml@main
    with:
      cloud-provider: azure
      tflint-ver: v0.52.0
      backend-type: s3
    secrets:
      azure-client-id: ${{ secrets.AZURE_CLIENT_ID }}
      azure-tenant-id: ${{ secrets.AZURE_TENANT_ID }}
      azure-subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
```

### Snowflake Deployment

```yaml
name: Deploy to Snowflake

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/.github/workflows/terraform-deploy.yaml@main
    with:
      cloud-provider: snowflake
      tflint-ver: v0.52.0
      backend-type: remote
      snowflake-account: myaccount
      snowflake-user: terraform_user
      snowflake-role: TERRAFORM_ROLE
    secrets:
      tfc-token: ${{ secrets.TF_CLOUD_TOKEN }}
      snowflake-private-key: ${{ secrets.SNOWFLAKE_PRIVATE_KEY }}
```

### Databricks Deployment

```yaml
name: Deploy to Databricks

on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/.github/workflows/terraform-deploy.yaml@main
    with:
      cloud-provider: databricks
      tflint-ver: v0.52.0
      backend-type: s3
    secrets:
      databricks-host: ${{ secrets.DATABRICKS_HOST }}
      databricks-token: ${{ secrets.DATABRICKS_TOKEN }}
```

## 🔧 Prerequisites

### For AWS
- Configure AWS OIDC provider in your GitHub repository
- Create an IAM role with necessary permissions
- Set up S3 bucket for Terraform state (if using S3 backend)

### For GCP
- Set up Workload Identity Federation
- Create a service account with required permissions
- Configure the WIF provider and service account

### For Azure
- Register an Azure AD application
- Create a service principal
- Assign necessary permissions to the service principal

### For Snowflake
- Create a Snowflake user for Terraform
- Generate RSA key pair for authentication
- Assign appropriate role and privileges

### For Databricks
- Create a Databricks workspace
- Generate a personal access token
- Configure workspace permissions

### For HCP Terraform Cloud
- Create a Terraform Cloud account
- Generate an API token
- Configure your workspace and variables

## 🛡️ Security Best Practices

- **Never commit secrets** to your repository
- Use **GitHub repository secrets** for all sensitive information
- Follow the **principle of least privilege** when setting up cloud permissions
- Regularly **rotate your credentials** and tokens
- Use **OIDC providers** where possible instead of long-lived credentials
- For Snowflake, use **key-pair authentication** instead of passwords
- For Databricks, use **service principals** or **OAuth tokens** when available

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

MIT

## 🆘 Support

If you encounter any issues or have questions:

1. Check the [Issues](https://github.com/subhamay-bhattacharyya-gha/tf-deploy-multi-reusable-wf/issues) page
2. Create a new issue with detailed information
3. Include workflow logs and error messages

---

**Built with ❤️ using [Kiro.dev](https://kiro.dev)**
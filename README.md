# 🛡️ Reference Architecture: Centralized Secrets Management with AWS Secrets Manager and Terraform

---

## 🔍 Overview
This reference architecture provides a secure, scalable, and standardized approach to managing application secrets (e.g., credentials, API keys, tokens) using **AWS Secrets Manager** and **Terraform**. It is designed to support cross-functional collaboration between engineering, DevOps, and compliance teams.

---

## 📅 Use Cases

- Engineering team stores DB credentials for production workloads.
- DevOps team manages GitHub tokens and pipeline secrets.
- Compliance team manages access to audit systems via API keys.

---

## 📏 Architecture Components

| Component                        | Description                                                               |
|----------------------------------|---------------------------------------------------------------------------|
| **AWS Secrets Manager**         | Stores encrypted secrets with fine-grained IAM access control.            |
| **Terraform**                   | Infrastructure-as-Code tool for declarative secret provisioning.          |
| **IAM Roles/Policies**          | Grants least-privilege access to secrets by team or application.          |
| **S3 + DynamoDB (Terraform State)** | Remote backend to securely store Terraform state files.                |
| **Lambda (optional)**           | Automates secret rotation for supported AWS services.                     |

---

## 📊 High-Level Design
Engineering/DevOps/Compliance Teams
|
| Use Terraform Modules
v
Terraform with Remote State (S3 + DynamoDB)
|
| Creates
v
Secrets in AWS Secrets Manager
|
| Accessed via IAM Policies
v
Apps / Pipelines / Audit Tools
```yaml

---

## 🔧 Example: Terraform Module Usage

```hcl
module "engineering_secrets" {
  source = "./modules/secret"

  name        = "engineering/prod/db_credentials"
  description = "DB creds for engineering"
  tags = {
    department = "engineering"
    environment = "prod"
  }

  secret_value = jsonencode({
    username = var.eng_db_user
    password = var.eng_db_pass
  })
}
```




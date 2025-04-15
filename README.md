# ☁️ Simple Time Service - Terraform Deployment

This project uses **Terraform** to deploy a containerized web application (Simple Time Service) on **AWS ECS Fargate** behind an **Application Load Balancer (ALB)**.

## 🚀 What This Project Does

- Creates a secure and scalable infrastructure on AWS using Terraform.
- Deploys a Docker image (`79975/simple-timeservice:latest`) from Docker Hub.
- Sets up networking (VPC, subnets, internet gateway, routing, security groups).
- Provisions ECS Fargate service with ALB for routing traffic.
- Exposes a simple web service that returns the current time.

## 📦 Prerequisites

- Terraform
- AWS CLI
- Docker (for local testing)
- An AWS account

## 🔐 Authentication

Before running Terraform, authenticate with AWS:

```bash
aws configure
```

## 🛠️ Usage

```bash
terraform init
terraform plan
terraform apply
```

After apply, Terraform will output a URL like:

```
load_balancer_url = "http://simpletimeservice-lb-xxx.us-west-2.elb.amazonaws.com"
```

## 📁 Structure

```
.
├── main.tf
├── variables.tf
├── terraform.tfvars
├── .gitignore
└── README.md
```

## 🧹 Cleanup

```bash
terraform destroy
```

## ✅ Acceptance Criteria

- ✅ One-step deployment via `terraform apply`
- ✅ App responds with current time
- ✅ No secrets committed
- ✅ Code uses variables and is documented

## 👨‍💻 Author

Built with ❤️ by kamalesh 

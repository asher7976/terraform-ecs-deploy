# ☁️ Simple Time Service - Terraform Deployment
This project uses Terraform to deploy a containerized web application (Simple Time Service) on AWS ECS Fargate behind an Application Load Balancer (ALB).

## 🚀 What This Project Does
This project creates a secure, scalable infrastructure on AWS using Terraform and deploys a Docker image from Docker Hub (`79975/simple-timeservice:latest`) into ECS Fargate.

### Features:
- Provisions networking (VPC, subnets, internet gateway, routing, security groups)
- Deploys ECS Fargate service with an ALB for routing traffic
- Exposes a simple web service that returns the current time

## 📦 Prerequisites

Before running Terraform, ensure you have the following installed:

- **Terraform** (v1.0+)
  - [Install Terraform](https://developer.hashicorp.com/terraform/downloads)
- **AWS CLI**
  - [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- **Docker** (for local testing)
  - [Install Docker](https://docs.docker.com/get-docker/)
- **AWS account** with necessary permissions to deploy resources (IAM user with ECS, EC2, ALB, IAM permissions)

### AWS Authentication

Before running Terraform, authenticate with AWS by configuring your AWS CLI credentials:

aws configure


You'll be prompted to enter:
- AWS Access Key ID
- AWS Secret Access Key
- Default Region Name
- Default Output Format

## 🛠️ Usage

Follow these steps to deploy the Simple Time Service:

1. **Initialize Terraform:**

    terraform init
  

2. **Review the plan:**

    terraform plan 
    

3. **Apply the Terraform configuration:**

    terraform apply     you type "yes" to apply the configuration here 
    After successful deployment, Terraform will output the following (example):

    load_balancer_url = "http://simpletimeservice-lb-abc123.us-west-2.elb.amazonaws.com"
    

4. **Test the service:**

    Use the provided URL in your browser or with `curl`:

    curl http://simpletimeservice-lb-abc123.us-west-2.elb.amazonaws.com
    
    Example Response:
    ```json
    { "current_time": "2025-04-15T12:45:33Z" }
    ```

## 📁 Project Structure


plaintext


.
├── main.tf              # Main infrastructure config: VPC, ECS, ALB, roles
├── variables.tf         # Input variables with descriptions and types
├── terraform.tfvars     # Actual values assigned to variables
├── outputs.tf           # Outputs like ALB DNS URL, ECS service name
├── providers.tf         # AWS provider config
├── .gitignore           # Ignore Terraform state & backups
└── README.md            # Project documentation
```

### Files:

- **`main.tf`**: Defines the infrastructure including VPC, subnets, ECS Fargate service, ALB, etc.
- **`variables.tf`**: Contains input variable definitions for flexibility and reuse.
- **`terraform.tfvars`**: Holds the values for the variables used in `variables.tf`.
- **`outputs.tf`**: Outputs key resources like the ALB DNS.
- **`providers.tf`**: AWS provider configuration for Terraform.

## 🧹 Cleanup
To destroy the deployed resources and avoid further costs, run:
  terraform destroy

This command will remove:
- ECS service and cluster
- Application Load Balancer (ALB)
- VPC, subnets, and other networking resources


## 🧠 Tips and Best Practices

- **Use variables** in `variables.tf` to manage configuration easily:
    ```hcl
    variable "region" {
      description = "AWS region to deploy resources"
      type        = string
      default     = "us-west-2"
    }
    ```

- **Sensitive Information**: Never hardcode secrets. Use AWS Secrets Manager, environment variables, or `.tfvars` files to keep them secure.



**Author**: Built with ❤️ by Kamalesh

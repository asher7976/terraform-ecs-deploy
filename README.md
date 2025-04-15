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

first clone this repo into ur local machine using the url:
  https://github.com/asher7976/terraform-ecs-deploy.git

cd terraform-ecs-deploy

1. **Initialize Terraform:**
     terraform init
  
2. **Review the plan:**
     terraform plan 
    
3. **Apply the Terraform configuration:**

    terraform apply    note( you type "yes" to apply the configuration here) 
    After successful deployment, Terraform will output the following (example):
   
    load_balancer_url = "http://simpletimeservice-lb-abc123.us-west-2.elb.amazonaws.com"
    

5. **Test the service:**

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

INFRASTRUCUTURE
1. Virtual Private Cloud (VPC)
A VPC provides the network boundary for all your resources. It’s a logically isolated section of AWS that you define. Inside the VPC, you'll have:

Subnets: The VPC will have one or more subnets (public and private). For this setup:

Public Subnet: Hosts the ALB (Application Load Balancer) which is internet-facing.

Private Subnet: Hosts the ECS Fargate tasks (your containers). These instances don't directly interact with the internet.

Internet Gateway (IGW): Provides access to/from the internet for resources in the public subnet (e.g., ALB).

Route Tables: Control the traffic routing inside the VPC and to/from the internet.

2. ECS Cluster and Fargate Service
Amazon ECS (Elastic Container Service) is a container orchestration service that runs and manages Docker containers. You are using ECS Fargate, which abstracts the underlying EC2 instances, so you don't need to manage servers directly.

ECS Cluster: A logical grouping of resources (tasks, services) where your containerized application will run.

ECS Fargate Task: Defines the container(s) that will run inside the ECS service. In your case, it’s a container running the 79975/simple-timeservice:latest image.

ECS Fargate Service: Ensures that the required number of ECS tasks (containers) are running and manages load balancing.

3. Application Load Balancer (ALB)
The ALB is responsible for routing HTTP(S) traffic to your ECS tasks based on the incoming request. It's set up with:

Listener: Listens for HTTP/HTTPS requests on the defined port (80 by default for HTTP).

Target Group: Routes the incoming traffic to ECS tasks. The target group is linked to your ECS service, so the load balancer directs traffic to the appropriate containers.

4. Security Groups
Security groups act as virtual firewalls for your EC2 instances and resources to control inbound and outbound traffic:

ALB Security Group: Allows inbound HTTP/HTTPS traffic (from the internet).

ECS Task Security Group: Allows inbound traffic only from the ALB to ensure that containers are not publicly accessible but can be accessed via the ALB.

5. IAM Roles and Policies
IAM (Identity and Access Management) is used to define permissions for your resources:

ECS Task Role: Allows the ECS tasks (your containers) to interact with other AWS services, such as pulling images from ECR or writing logs to CloudWatch.

ECS Service Role: Grants ECS permission to manage resources on your behalf (e.g., creating and managing tasks).

ALB Role: Manages permissions for the ALB to interact with ECS and forward traffic.

6.Terraform Resources
These Terraform resources and modules will be used to create the infrastructure:

aws_vpc: Creates the VPC with specified CIDR blocks.

aws_subnet: Creates the public and private subnets.

aws_internet_gateway: Creates the internet gateway to allow internet traffic.

aws_security_group: Defines rules for controlling access to the ECS tasks and ALB.

aws_lb: The Application Load Balancer for routing HTTP(S) traffic.

aws_lb_target_group: Associates the ECS tasks with the ALB.

aws_ecs_cluster: The ECS cluster to run the Fargate tasks.

aws_ecs_task_definition: Defines the Docker container and how it should run.

aws_ecs_service: Manages the ECS tasks and ensures they run and scale.


## 🧠 Tips and Best Practices

- **Use variables** in `variables.tf` to manage configuration easily:
    
    variable "region" {
      description = "AWS region to deploy resources"
      type        = string
      default     = "us-west-2"
    }
    ```

- **Sensitive Information**: Never hardcode secrets. Use AWS Secrets Manager, environment variables, or `.tfvars` files to keep them secure.



**Author**: Built with ❤️ by Kamalesh

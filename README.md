# Terraform Training Guide for Beginners

## 1. What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and provision infrastructure using declarative configuration files. Instead of manually setting up servers, networks, and other resources through cloud provider interfaces, you define your infrastructure in code and let Terraform handle the deployment.

## 2. Core Concepts

### a) Providers
Providers are plugins that allow Terraform to interact with cloud platforms (AWS, Azure, GCP), services (GitHub, DataDog), or other APIs. Example:

```hcl
provider "aws" {
  region = "us-west-2"
}
```

### b) Resources
Resources are the most important element in Terraform. They describe infrastructure objects like virtual networks, compute instances, or DNS records. Example:

```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "Web Server"
  }
}
```

### c) Variables
Variables make your Terraform configurations more flexible and reusable. Example:

```hcl
variable "environment" {
  description = "Environment (dev/staging/prod)"
  type        = string
  default     = "dev"
}

resource "aws_instance" "web_server" {
  tags = {
    Environment = var.environment
  }
}
```

## 3. Basic Workflow

### Initialize the Project
```bash
terraform init
```
This downloads required providers and initializes the working directory.

### Plan the Changes
```bash
terraform plan
```
This shows what changes Terraform will make to your infrastructure.

### Apply Changes
```bash
terraform apply
```
This applies the changes to create/modify your infrastructure.

### Destroy Infrastructure
```bash
terraform destroy
```
This removes all resources managed by Terraform.

## 4. Best Practices

### State Management
- Store state files remotely (e.g., S3, Azure Storage)
- Use state locking to prevent concurrent modifications
- Never commit state files to version control

Example backend configuration:
```hcl
terraform {
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "prod/terraform.tfstate"
    region = "us-west-2"
  }
}
```

### Code Organization
- Use modules for reusable components
- Separate environments using workspaces or directory structure
- Follow consistent naming conventions

### Example Module Structure
```
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── ec2/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── environments/
│   ├── dev/
│   │   └── main.tf
│   └── prod/
│       └── main.tf
└── terraform.tfvars
```

## 5. Hands-on Exercise

Start with a simple example that creates an AWS S3 bucket:

```hcl
# main.tf
provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "demo_bucket" {
  bucket = "my-terraform-demo-bucket"
  
  tags = {
    Environment = "Training"
    Manager     = "Terraform"
  }
}

# Show how to enable versioning
resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.demo_bucket.id
  versioning_configuration {
    status = "Enabled"
  }
}
```

## 6. Common Commands Reference

```bash
# Initialize working directory
terraform init

# Format code
terraform fmt

# Validate configuration
terraform validate

# Show planned changes
terraform plan

# Apply changes
terraform apply

# Destroy resources
terraform destroy

# Show current state
terraform show

# List resources in state
terraform state list
```

## 7. Next Steps
After mastering the basics, explore:
- Remote state management
- Workspace usage
- Complex module creation
- CI/CD integration
- Import existing infrastructure
- Data sources and provisioners

Remember to always refer to the official Terraform documentation for the most up-to-date information and best practices.

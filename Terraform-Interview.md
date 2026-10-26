# Terraform Interview Questions and Answers

## Table of Contents
1. [Basic Level](#basic-level)
2. [Intermediate Level](#intermediate-level)
3. [Advanced Level](#advanced-level)

---

## Basic Level

### 1. What is Terraform?
**Answer:**
Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define, preview, and deploy infrastructure across multiple cloud providers and on-premises environments. It uses a declarative configuration language (HCL) to describe your infrastructure.

**Key Features:**
- Declarative syntax
- Multi-cloud support (AWS, Azure, GCP, etc.)
- State management
- Plan and Apply workflow
- Modular infrastructure code

---

### 2. What is HCL and why is it used in Terraform?
**Answer:**
HCL (HashiCorp Configuration Language) is a language designed for humans to write infrastructure code. It's used in Terraform because:
- Easy to read and understand
- Supports nested structures
- Compatible with JSON
- Allows comments
- Provides a balance between simplicity and power

**Example:**
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "web-server"
  }
}
```

---

### 3. What is Terraform State?
**Answer:**
Terraform State is a file (terraform.tfstate) that Terraform uses to map your configuration to real-world resources. It stores:
- Resource metadata
- Resource IDs
- Relationships between resources
- Current attribute values

**Important Points:**
- Should be stored in version control with care (contains sensitive data)
- Can be stored remotely (S3, Terraform Cloud, etc.)
- Lock mechanism prevents concurrent modifications
- Backup is automatically created (terraform.tfstate.backup)

---

### 4. What are the main Terraform commands?
**Answer:**
| Command | Purpose |
|---------|---------|
| `terraform init` | Initialize a Terraform working directory |
| `terraform validate` | Check configuration syntax |
| `terraform plan` | Create an execution plan |
| `terraform apply` | Execute the plan |
| `terraform destroy` | Destroy all resources |
| `terraform fmt` | Format code |
| `terraform state` | Manage state |

---

### 5. What is the difference between Terraform and CloudFormation?
**Answer:**

| Feature | Terraform | CloudFormation |
|---------|-----------|----------------|
| **Cloud Support** | Multi-cloud | AWS only |
| **Language** | HCL | JSON/YAML |
| **State Management** | Explicit state file | AWS managed |
| **Learning Curve** | Moderate | AWS specific |
| **Community** | Larger | AWS focused |
| **Cost** | Free (open-source) | Free (AWS service) |

---

### 6. What are variables in Terraform and how do you define them?
**Answer:**
Variables allow you to parameterize your configuration making it reusable and flexible.

**Definition:**
```hcl
variable "instance_count" {
  description = "Number of instances to create"
  type        = number
  default     = 1
}

variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-1"
}

variable "tags" {
  description = "Common tags"
  type        = map(string)
  default = {
    Environment = "dev"
    Owner       = "team"
  }
}
```

**Types:** string, number, bool, list, map, object, tuple, any

---

### 7. What are outputs in Terraform?
**Answer:**
Outputs are values that you want to expose from your Terraform configuration. They are useful for:
- Getting important values after apply
- Passing values between modules
- Using values in other configurations

**Definition:**
```hcl
output "instance_id" {
  description = "ID of the instance"
  value       = aws_instance.web.id
  sensitive   = false
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

### 8. What are resources in Terraform?
**Answer:**
Resources are the infrastructure objects you want to create and manage (EC2 instances, RDS databases, S3 buckets, etc.).

**Syntax:**
```hcl
resource "provider_type" "resource_name" {
  argument1 = value1
  argument2 = value2
}
```

**Example:**
```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "my-unique-bucket-name"
  acl    = "private"
}
```

---

### 9. What is a data source in Terraform?
**Answer:**
Data sources allow you to fetch information from existing infrastructure without managing it. They are read-only.

**Difference from Resources:**
- Resources: Create/manage infrastructure
- Data sources: Read existing infrastructure

**Example:**
```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}
```

---

### 10. What is a provider in Terraform?
**Answer:**
Providers are plugins that Terraform uses to communicate with APIs of cloud platforms and services.

**Common Providers:** AWS, Azure, GCP, Kubernetes, Docker, GitHub, etc.

**Configuration:**
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}
```

---

## Intermediate Level

### 11. What are modules in Terraform and why should you use them?
**Answer:**
Modules are containers for multiple resources that are used together. They help:
- Organize code
- Promote reusability
- Improve maintainability
- Simplify complex configurations

**Module Structure:**
```
my-module/
├── main.tf
├── variables.tf
├── outputs.tf
└── terraform.tfvars
```

**Usage:**
```hcl
module "vpc" {
  source = "./modules/vpc"
  
  vpc_cidr = "10.0.0.0/16"
  region   = "us-east-1"
}
```

---

### 12. What is Remote State and why is it important?
**Answer:**
Remote State stores the Terraform state file in a centralized location (not locally) making it accessible to multiple team members.

**Advantages:**
- Collaboration among team members
- Better security (state contains sensitive data)
- Automatic backups
- State locking prevents concurrent modifications
- Easier CI/CD integration

**Popular Backends:**
- Terraform Cloud/Enterprise
- AWS S3 with DynamoDB for locking
- Azure Storage Account
- Google Cloud Storage
- HashiCorp Consul

**Configuration:**
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

---

### 13. What is terraform.tfvars and how is it used?
**Answer:**
terraform.tfvars is a file that contains variable values. It allows you to set variable values without modifying the main configuration.

**Usage:**
```hcl
# variables.tf
variable "aws_region" {
  type = string
}

variable "instance_count" {
  type = number
}

# terraform.tfvars
aws_region    = "us-west-2"
instance_count = 3
```

**Ways to Pass Variables:**
1. terraform.tfvars (automatically loaded)
2. *.auto.tfvars (automatically loaded)
3. `-var` flag: `terraform apply -var="region=us-west-2"`
4. Environment variables: `TF_VAR_region`

---

### 14. What are count and for_each and when to use them?
**Answer:**
Both allow you to create multiple instances of resources.

**count:**
```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "instance-${count.index}"
  }
}
```

**for_each:**
```hcl
variable "instances" {
  type = map(string)
  default = {
    prod  = "t2.large"
    dev   = "t2.micro"
    test  = "t2.small"
  }
}

resource "aws_instance" "web" {
  for_each      = var.instances
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = each.value
  tags = {
    Name = each.key
  }
}
```

**When to Use:**
- `count`: Simple iterations with index
- `for_each`: Map/set iterations, better for changes

---

### 15. What is Terraform import and when is it used?
**Answer:**
Terraform import brings existing infrastructure under Terraform management.

**Use Cases:**
- Migrating infrastructure to Terraform
- Managing manually created resources
- Taking over infrastructure management

**Command:**
```bash
terraform import aws_instance.web i-1234567890abcdef0
```

**Limitations:**
- Import doesn't create configuration file (you must write it)
- Only creates state entry
- Complex resources may require manual adjustment

---

### 16. What is the difference between terraform plan and terraform apply?
**Answer:**

| Aspect | terraform plan | terraform apply |
|--------|---|---|
| **Purpose** | Preview changes | Execute changes |
| **Modifies State** | No | Yes |
| **Modifies Infrastructure** | No | Yes |
| **Generates Execution Plan** | Yes | Yes (then executes) |
| **Approval** | Not needed | Can require approval |

**Workflow:**
```bash
terraform plan -out=plan.tfplan    # Preview changes
terraform apply plan.tfplan        # Execute changes
```

---

### 17. How do you manage secrets in Terraform?
**Answer:**
Best practices for managing secrets:

**1. Sensitive Variables:**
```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

**2. Use Terraform Cloud/Enterprise:**
- Store secrets in remote backend
- Never exposed in logs

**3. External Secret Management:**
```hcl
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "prod/db/password"
}

resource "aws_db_instance" "default" {
  password = data.aws_secretsmanager_secret_version.db_password.secret_string
}
```

**4. Avoid:**
- Storing secrets in version control
- Using plaintext in code
- Exposing in terraform.tfvars

---

### 18. What is a workspace in Terraform?
**Answer:**
Workspaces allow you to manage multiple environments (dev, staging, prod) with the same configuration.

**Commands:**
```bash
terraform workspace new dev
terraform workspace new prod
terraform workspace select dev
terraform workspace list
```

**Usage:**
```hcl
locals {
  environment = terraform.workspace
}

resource "aws_instance" "web" {
  instance_type = terraform.workspace == "prod" ? "t2.large" : "t2.micro"
  tags = {
    Environment = local.environment
  }
}
```

**Limitations:**
- Workspaces are stored in the same backend
- Not ideal for complete isolation
- Better alternative: Separate directories/modules

---

### 19. What is the purpose of terraform fmt?
**Answer:**
`terraform fmt` formats Terraform configuration files to a canonical format.

**Purpose:**
- Enforce consistent code style
- Improve readability
- Standardize formatting across team

**Usage:**
```bash
terraform fmt                    # Format current directory
terraform fmt -recursive         # Format all directories
terraform fmt -check           # Check without modifying
```

---

### 20. How do you handle resource dependencies in Terraform?
**Answer:**
Terraform automatically infers dependencies from resource references. You can also explicitly define them.

**Implicit Dependencies:**
```hcl
resource "aws_instance" "web" {
  subnet_id = aws_subnet.main.id  # Implicit dependency
}

resource "aws_subnet" "main" {
  vpc_id = aws_vpc.main.id
}
```

**Explicit Dependencies:**
```hcl
resource "aws_instance" "web" {
  depends_on = [aws_internet_gateway.main]
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
}
```

---

## Advanced Level

### 21. What is Terraform Cloud and what are its benefits?
**Answer:**
Terraform Cloud is a hosted platform by HashiCorp for collaborative infrastructure management.

**Benefits:**
- Remote state storage with locking
- VCS integration (GitHub, GitLab, Bitbucket)
- Runs triggered by VCS changes
- Collaboration features
- Policy as Code (Sentinel)
- Cost estimation
- Team management
- Audit logs

**Pricing:**
- Free tier available
- Paid tiers for organizations

---

### 22. What is Sentinel and how is it used in Terraform?
**Answer:**
Sentinel is a policy-as-code framework for defining and enforcing rules.

**Use Cases:**
- Enforce resource naming conventions
- Prevent certain resource types
- Enforce cost limits
- Compliance checks
- Security policies

**Example Policy:**
```python
# require-vpc-for-ec2.sentinel
import "tfplan/v2" as tfplan

deny_list = [
  "aws_instance",
]

main = rule {
  all tfplan.resources[deny_list[_]] as _, instances {
    all instances as _, config {
      config.change.after.subnet_id != null
    }
  }
}
```

---

### 23. How do you handle complex module dependencies?
**Answer:**
Strategies for complex module interactions:

**1. Module Composition:**
```hcl
module "vpc" {
  source = "./modules/vpc"
}

module "security_groups" {
  source = "./modules/security_groups"
  vpc_id = module.vpc.vpc_id
}

module "rds" {
  source = "./modules/rds"
  vpc_id = module.vpc.vpc_id
  security_group_id = module.security_groups.rds_sg_id
}
```

**2. Output Dependencies:**
```hcl
# modules/rds/main.tf
output "rds_endpoint" {
  value = aws_db_instance.main.endpoint
}

# main.tf
module "app" {
  source = "./modules/app"
  database_url = module.rds.rds_endpoint
}
```

**3. Use locals for organizing:**
```hcl
locals {
  vpc_config = module.vpc
  security_config = module.security_groups
}
```

---

### 24. What are provisioners and when should they be used?
**Answer:**
Provisioners run actions on resources during creation or destruction.

**Types:**
- `local-exec`: Run on local machine
- `remote-exec`: Run on remote resource

**Example:**
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx"
    ]
    
    connection {
      type        = "ssh"
      user        = "ubuntu"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip
    }
  }
}
```

**When to Use:**
- Last resort for configuration
- Use user_data instead when possible
- Use configuration management tools (Ansible, Chef, Puppet)

**Limitations:**
- Not idempotent by default
- Hard to manage failures
- Makes infrastructure less reproducible

---

### 25. How do you implement dynamic blocks in Terraform?
**Answer:**
Dynamic blocks allow you to generate nested blocks dynamically.

**Syntax:**
```hcl
dynamic "block_name" {
  for_each = var.list_variable
  content {
    key = block_name.value
  }
}
```

**Example:**
```hcl
variable "security_group_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
  }))
}

resource "aws_security_group" "main" {
  name = "main"

  dynamic "ingress" {
    for_each = var.security_group_rules
    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}
```

---

### 26. What is the difference between local and remote execution environments?
**Answer:**

**Local Execution:**
- Terraform runs on your machine
- State file stored locally
- Suitable for small projects

**Remote Execution:**
- Terraform runs on Terraform Cloud/Enterprise
- State managed centrally
- Better for team collaboration
- VCS-driven workflows
- More secure and auditable

**Advantages of Remote Execution:**
- No local state files
- Consistent environment
- Audit trails
- Policy enforcement
- Better security

---

### 27. How do you optimize Terraform performance for large configurations?
**Answer:**

**1. Use Refresh False:**
```bash
terraform plan -refresh=false
terraform apply -refresh=false
```

**2. Target Specific Resources:**
```bash
terraform plan -target=aws_instance.web
```

**3. Parallelize Operations:**
```bash
terraform apply -parallelism=10
```

**4. Use Local Exec Caching:**
```bash
terraform apply -input=false -no-color
```

**5. Optimize Module Structure:**
- Separate by environment
- Use data sources efficiently
- Avoid unnecessary loops

**6. State Management:**
- Use remote state
- Keep state files small
- Archive old workspaces

---

### 28. How do you implement a multi-environment setup with Terraform?
**Answer:**

**Directory Structure:**
```
terraform/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── terraform.tfvars
│   ├── staging/
│   └── prod/
└── modules/
    ├── vpc/
    ├── rds/
    └── app/
```

**Environment Configuration:**
```hcl
# environments/dev/terraform.tfvars
environment  = "dev"
instance_type = "t2.micro"
database_size = "db.t2.micro"

# environments/prod/terraform.tfvars
environment  = "prod"
instance_type = "t2.large"
database_size = "db.r5.large"
```

**Main Configuration:**
```hcl
# environments/dev/main.tf
module "vpc" {
  source = "../../modules/vpc"
  environment = var.environment
  cidr_block = "10.0.0.0/16"
}

module "rds" {
  source = "../../modules/rds"
  environment = var.environment
  instance_class = var.database_size
}
```

---

### 29. What is Terraform Cloud/Enterprise's run queue and how does it work?
**Answer:**
The run queue manages the execution of Terraform operations in Terraform Cloud.

**How It Works:**
1. VCS commit triggers a run
2. Run enters the queue
3. Terraform performs plan
4. Waits for confirmation
5. Executes apply

**Features:**
- Serial execution by default
- Can be configured for parallel runs
- Prevents concurrent modifications
- Provides audit trail

---

### 30. How do you handle stateful data migration when refactoring Terraform?
**Answer:**
Techniques for refactoring without losing state:

**1. State Mv Command:**
```bash
terraform state mv aws_instance.old aws_instance.new
terraform state mv 'module.old' 'module.new'
```

**2. Manual State Editing:**
```bash
terraform state pull > terraform.tfstate.backup
# Edit terraform.tfstate carefully
terraform state push terraform.tfstate
```

**3. Moved Blocks (Terraform 1.1+):**
```hcl
moved {
  from = aws_instance.old
  to   = aws_instance.new
}
```

**4. Destroy and Recreate:**
```bash
terraform destroy -target=aws_instance.old
# Update configuration
terraform apply
```

**Best Practice:**
- Always backup state before migration
- Test in non-production first
- Use version control for tracking changes
- Document migration steps

---

### 31. What are best practices for Terraform code organization?
**Answer:**

**1. Directory Structure:**
```
project/
├── modules/              # Reusable modules
│   ├── vpc/
│   ├── rds/
│   └── app/
├── environments/         # Environment-specific configs
│   ├── dev/
│   ├── staging/
│   └── prod/
├── .terraform/          # Downloaded providers
├── .gitignore
├── terraform.tfstate    # State file
└── README.md
```

**2. File Naming Convention:**
```
main.tf           # Main configuration
variables.tf      # Variable definitions
outputs.tf        # Output definitions
provider.tf       # Provider configuration
locals.tf         # Local values
data.tf           # Data sources
```

**3. Module Structure:**
```
module/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

**4. Code Style:**
- Use consistent indentation (2 spaces)
- Use descriptive resource names
- Add comments for complex logic
- Use terraform fmt regularly

**5. Documentation:**
- README for each module
- Document variables and outputs
- Include usage examples

---

### 32. How do you implement CI/CD with Terraform?
**Answer:**

**Basic Pipeline:**
```yaml
# .github/workflows/terraform.yml
name: Terraform CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - uses: hashicorp/setup-terraform@v1
        with:
          terraform_version: 1.0.0
      
      - name: Terraform Init
        run: terraform init
      
      - name: Terraform Validate
        run: terraform validate
      
      - name: Terraform Format
        run: terraform fmt -check
      
      - name: Terraform Plan
        run: terraform plan -out=plan.tfplan
      
      - name: Terraform Apply
        if: github.event_name == 'push'
        run: terraform apply -auto-approve plan.tfplan
```

**Best Practices:**
- Separate plan and apply steps
- Require approval for production
- Use branches for testing
- Store state in remote backend
- Implement policy checks with Sentinel

---

### 33. What are common Terraform errors and how to troubleshoot them?
**Answer:**

**Common Errors:**

| Error | Cause | Solution |
|-------|-------|----------|
| **State Lock** | Concurrent runs | Wait or use `force-unlock` |
| **Resource Already Exists** | Import conflict | Use `terraform import` |
| **Syntax Error** | Invalid HCL | Use `terraform validate` |
| **Plan Failed** | Invalid config | Check credentials, permissions |
| **Drift Detection** | Manual changes | Use `terraform refresh` |

**Debugging:**
```bash
terraform validate              # Check syntax
terraform plan -out=plan.tfplan # Preview changes
terraform apply -parallelism=1  # Single threaded
TF_LOG=DEBUG terraform apply    # Enable logging
```

---

### 34. What is terraform-compliance and how is it used?
**Answer:**
terraform-compliance is a tool for testing Terraform code against compliance and security policies.

**Use Case:**
```bash
terraform-compliance -p policies/ -f terraform/
```

**Example Policy:**
```gherkin
@network
Scenario: Ensure all security groups are in a VPC
  Given I have aws_security_group defined
  When it includes id
  Then its vpc_id must not be empty
```

---

### 35. How do you handle cross-stack dependencies in Terraform?
**Answer:**

**Methods:**

**1. Output References:**
```hcl
# stack1/outputs.tf
output "vpc_id" {
  value = aws_vpc.main.id
}

# stack2/main.tf
data "terraform_remote_state" "vpc_stack" {
  backend = "s3"
  config = {
    bucket = "my-state"
    key    = "stack1/terraform.tfstate"
  }
}

resource "aws_subnet" "main" {
  vpc_id = data.terraform_remote_state.vpc_stack.outputs.vpc_id
}
```

**2. Module Composition:**
```hcl
module "vpc" { ... }
module "app" {
  vpc_id = module.vpc.vpc_id
}
```

**3. Shared State:**
- Use same backend
- Combine stacks into single state

---

### 36. What is the difference between var, local, and output?
**Answer:**

| Feature | variable | local | output |
|---------|----------|-------|--------|
| **Purpose** | Input parameters | Intermediate values | Expose values |
| **Scope** | Configuration-wide | Module/block-scoped | External visibility |
| **Modification** | User provides | Computed | Read-only |
| **Examples** | `var.instance_type` | `local.tag_name` | `output.instance_id` |

**Example:**
```hcl
variable "environment" {
  type = string
}

locals {
  full_name = "${var.environment}-resource"
}

output "resource_name" {
  value = local.full_name
}
```

---

### 37. How do you implement infrastructure testing with Terraform?
**Answer:**

**Testing Frameworks:**

**1. Terratest (Go-based):**
```go
package test

import (
	"testing"
	"github.com/gruntwork-io/terratest/modules/terraform"
)

func TestTerraformExample(t *testing.T) {
	opts := &terraform.Options{
		TerraformDir: "../examples",
	}
	defer terraform.Destroy(t, opts)
	terraform.InitAndApply(t, opts)
	
	instanceID := terraform.Output(t, opts, "instance_id")
	// Assertions...
}
```

**2. Checkov:**
```bash
checkov -d terraform/ --framework terraform
```

**3. Tflint:**
```bash
tflint
```

---

### 38. What is the lifecycle block in Terraform and its use cases?
**Answer:**
Lifecycle blocks control how Terraform handles resource creation, modification, and deletion.

**Arguments:**

```hcl
resource "aws_instance" "example" {
  # ...
  
  lifecycle {
    create_before_destroy = true
    ignore_changes        = [tags["Date"]]
    prevent_destroy       = true
  }
}
```

**Use Cases:**
- `create_before_destroy`: Blue-green deployments
- `ignore_changes`: Ignore specific attribute changes
- `prevent_destroy`: Prevent accidental deletion
- `replace_triggered_by`: Trigger replacement on changes

---

### 39. How do you handle Terraform upgrades safely?
**Answer:**

**Upgrade Steps:**
```bash
# 1. Backup state
terraform state pull > backup.tfstate

# 2. Check upgrade path
terraform -version
# Check changelog for breaking changes

# 3. Test upgrade in dev
terraform init -upgrade
terraform plan

# 4. Review and apply
terraform apply

# 5. Verify
terraform validate
terraform plan -out=/dev/null  # Ensure empty plan
```

**Best Practices:**
- Test in non-production first
- Review breaking changes
- Update provider versions gradually
- Maintain compatibility
- Document changes

---

### 40. What are common Terraform security best practices?
**Answer:**

**1. State Management:**
```hcl
terraform {
  backend "s3" {
    encrypt = true
    dynamodb_table = "terraform-locks"
  }
}
```

**2. Secrets Management:**
```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

**3. Access Control:**
- Use IAM roles (not access keys)
- Implement least privilege
- Enable MFA
- Audit logging

**4. Code Security:**
```bash
# Scan for secrets
tfsec terraform/

# Scan for compliance
checkov -d terraform/
```

**5. Network Security:**
```hcl
resource "aws_security_group" "app" {
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]  # Consider restricting
  }
}
```

**6. Infrastructure:**
- Enable versioning on state buckets
- Use encryption at rest
- Enable MFA delete
- Backup state regularly
- Use VPC endpoints

---

## Summary

This guide covers Terraform from basics to advanced topics. Key takeaways:

1. **Master the basics**: HCL syntax, state management, plan/apply workflow
2. **Understand modules**: Critical for code reusability and organization
3. **Implement best practices**: Security, testing, CI/CD
4. **Use advanced features**: Workspaces, dynamic blocks, provisioners
5. **Focus on collaboration**: Remote state, Terraform Cloud, team workflows
6. **Test thoroughly**: Use testing frameworks and policy enforcement
7. **Document everything**: Code and infrastructure changes

Good luck with your Terraform interview!

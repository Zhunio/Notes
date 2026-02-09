# Setup Terraform Bastion Host

This guide will help you set up a Bastion host using Terraform on AWS.

## Prequisites

- [Setup IAM User for Terraform](setup-terraform-iam-user.md)

## Generate SSH key pair for Bastion Host

```sh
ssh-keygen -t rsa -b 4096 -f ~/.ssh/terraform-bastion-key
```

## Create infrastructure using Terraform

#### 1. Create the following file structure:

```sh
sample-project
├─ infrastructure        # code for infrastructure
│  └─ bastion            # code to create bastion host
│     └─ main.tf
└─ ... other files
```

#### 2. Create Bastion Host Terraform Configuration

```terraform
# infrastructure/bastion/main.tf

variable "my_public_ip" {
  description = "Your public IP address to allow SSH access to the bastion host"
  type        = string
}

locals {
  # 💀 Replace <project-name> with your project name
  project_name                = "<project-name>"
  # Name of the bastion key pair
  bastion_key_name            = "terraform-${local.project_name}-bastion-key"
  # Name of the bastion security group
  bastion_security_group_name = "terraform-${local.project_name}-bastion-security-group"
  # Name of the bastion instance key
  bastion_instance_key_name   = "terraform-${local.project_name}-bastion-instance"
}

data "aws_vpc" "default" {
  default = true
}

data "aws_subnets" "default" {
  filter {
    name   = "vpc-id"
    values = [data.aws_vpc.default.id]
  }
}

data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

resource "aws_key_pair" "bastion_key" {
  key_name   = local.bastion_key_name
  public_key = file("~/.ssh/terraform-bastion-key.pub")
}

resource "aws_security_group" "bastion_security_group" {
  name   = local.bastion_security_group_name
  vpc_id = data.aws_vpc.default.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["${var.my_public_ip}/32"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "bastion_instance" {
  ami                         = data.aws_ami.amazon_linux.id
  subnet_id                   = data.aws_subnet_ids.default.ids[0]
  key_name                    = local.bastion_instance_key_name
  security_groups             = [aws_security_group.bastion.id]
  instance_type               = "t3.nano"
  associate_public_ip_address = true
}

output "bastion_public_ip" {
  description = "Public IP address of the bastion host"
  value       = aws_instance.bastion_instance.public_ip
}

```

## Define my_public_ip Environment Variable

```sh
export TF_VAR_my_public_ip=$(curl -s ifconfig.me)
```

## Apply Terraform

1. Create a new directory for your Terraform configuration files.

```sh
cd infrastructure/bastion
terraform init
terraform apply -auto-approve
```

## Check Public IP

- `bastion_public_ip`: The public IP address of the Bastion host.

## Notes

- Replace `YOUR_PUBLIC_IP` with your actual public IP address (without `/32`).
- Ensure your SSH key exists at the specified path before applying.

```

```

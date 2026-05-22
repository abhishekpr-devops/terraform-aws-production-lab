# Terraform AWS Production Lab

A production-style Terraform project for building and managing AWS infrastructure using Terraform.

This project demonstrates Infrastructure as Code concepts such as providers, resources, variables, outputs, locals, modules, remote state, state locking, lifecycle protection, drift detection, import, and version pinning.

## Project Goals

- Build AWS infrastructure using Terraform
- Manage cloud resources through code
- Use reusable Terraform modules
- Store Terraform state remotely in S3
- Enable S3 state locking
- Detect and fix infrastructure drift
- Import existing AWS resources into Terraform
- Keep the GitHub repository safe from secrets and state files

## AWS Resources Used

- VPC
- Public subnet
- Internet gateway
- Route table
- Security group
- EC2 web server
- S3 demo bucket
- Imported S3 bucket
- S3 backend bucket for remote state
- S3 lockfile for state locking

## Architecture Overview

Internet users access an EC2 web server running inside a public subnet. The EC2 instance is protected by a security group. The subnet belongs to a VPC and reaches the internet through an internet gateway and route table.

Terraform state is stored remotely in an S3 backend with lockfile support enabled.

## Project Structure

terraform-lab/
├── environments/
│   ├── dev/
│   ├── staging/
│   └── prod/
├── modules/
│   ├── ec2/
│   ├── network/
│   └── security/
├── shared/
├── .gitignore
└── README.md

## Security Notes

This repository is prepared to be safe for GitHub.

The following files are ignored:

- Terraform state files
- Terraform backup state files
- Real terraform.tfvars files
- Real backend.hcl files
- Screenshots
- Local documentation
- Generated graph files
- Local notes

Only example files are committed:

- terraform.tfvars.example
- backend.hcl.example

## How to Use

Go to the dev environment:

    cd environments/dev

Create a real variable file:

    cp terraform.tfvars.example terraform.tfvars

Create a real backend config file:

    cp backend.hcl.example backend.hcl

Initialize Terraform:

    terraform init -backend-config=backend.hcl

Validate:

    terraform validate

Preview changes:

    terraform plan

Apply changes:

    terraform apply

## EC2 Module

The EC2 instance is managed through a reusable module located at:

    modules/ec2/

The module is called from the dev environment using:

    module "web_server" {
      source = "../../modules/ec2"
    }

## Key Terraform Concepts Practiced

| Concept | Meaning |
|---|---|
| Provider | Connects Terraform to AWS |
| Resource | Cloud object managed by Terraform |
| Variable | Reusable input value |
| Output | Value displayed after apply |
| Local | Internal reusable value |
| State | Terraform memory of infrastructure |
| Remote Backend | Stores state outside local machine |
| Locking | Prevents simultaneous state writes |
| Module | Reusable Terraform component |
| Workspace | Separate state environment |
| Lifecycle Rule | Controls resource behavior |
| Drift | Difference between code and real AWS |
| Import | Bring existing infrastructure into Terraform |
| Version Pinning | Controls Terraform/provider versions |

## Lifecycle Protection

The demo S3 bucket uses prevent_destroy to protect it from accidental deletion.

## Drift Detection

Drift was tested by manually changing an EC2 tag in the AWS Console. Terraform detected the change using terraform plan and repaired it using terraform apply.

## Importing Existing Infrastructure

An existing S3 bucket was imported into Terraform using terraform import.

## Version Pinning

Terraform version is pinned to:

    required_version = "~> 1.15"

AWS provider version is pinned to:

    version = "~> 6.0"

## Cost Warning

This project creates real AWS resources. To avoid charges, stop or destroy resources when not needed and review AWS Billing regularly.

Do not delete the backend state bucket before cleanup because Terraform needs the state file to know what it manages.

## Author

Created by abhishekpr-devops as a Terraform AWS production-style hands-on lab.

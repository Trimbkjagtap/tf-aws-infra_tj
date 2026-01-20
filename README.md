# Infrastructure Setup Guide

This guide explains how to set up and manage cloud infrastructure using Terraform and AWS CLI.

---

## Prerequisites

Before starting, ensure you have the following packages installed:

- **AWS CLI**
- **Terraform**

---

## AWS CLI Configuration

AWS CLI is used to interact with Amazon Web Services. Follow these steps to configure it properly.

### Create AWS CLI Profile

Use the following command to create a new AWS CLI profile:
```bash
aws configure --profile profile_name
```

You will be prompted to enter:
- AWS Access Key ID
- AWS Secret Access Key
- Default region name
- Default output format

### Set AWS Profile as Environment Variable

To use the configured profile, you need to set it as a temporary environment variable in your system.

**Windows:**
```bash
set AWS_PROFILE=profile_name
```

**Linux / macOS:**
```bash
export AWS_PROFILE=profile_name
```

> **Note:** You must set the AWS profile before using Terraform commands.

---

## Setting Up Infrastructure Using Terraform

Terraform is used to provision and manage cloud infrastructure as code.

### Terraform Commands

Below are the essential Terraform commands and their purposes:

#### Initialize Terraform
```bash
terraform init
```
This command installs required plugins and sets up the working directory containing configuration files. Run this **only for the first time** or when adding new providers.

#### Validate Configuration
```bash
terraform validate
```
This command checks whether the configuration files are syntactically valid.

#### Format Configuration Files
```bash
terraform fmt
```
This command formats configuration files with correct indentation and styling.

#### Plan Infrastructure Changes
```bash
terraform plan
```
This command shows the changes required by the current configuration. It's a preview of what will be created, modified, or destroyed.

#### Apply Configuration
```bash
terraform apply
```
This command applies the configuration to create or update infrastructure.

#### Destroy Infrastructure
```bash
terraform destroy
```
This command destroys all infrastructure created by Terraform.

---

## Running Terraform with Variable Files

When working with variable files (`.tfvars`), you can pass them directly using the `-var-file` option.

### Minimum Commands Required

1. **Initialize (first time only):**
```bash
   terraform init
```

2. **Plan (optional but recommended):**
```bash
   terraform plan -var-file=file_name.tfvars
```

3. **Apply configuration:**
```bash
   terraform apply -var-file=file_name.tfvars
```

4. **Destroy infrastructure (when done):**
```bash
   terraform destroy -var-file=file_name.tfvars
```

---

## Infrastructure Created by Terraform

Running this Terraform configuration will provision the following AWS resources:

### VPC (Virtual Private Cloud)
- Creates a VPC with public and private subnets
- Configures routing tables and internet gateway

### IAM Roles and Policies
- Creates IAM roles and policies for EC2 instances
- Enables the application running on EC2 to call AWS services securely

### AWS RDS Instance
- Creates a managed relational database instance
- Attaches security groups to allow EC2 instance access
- Configures database with necessary parameters

### AWS EC2 Instance
- Creates EC2 instances using custom AMI (Amazon Machine Image)
- AMI is provided through the variable file
- Configures security groups for proper access control

### AWS S3 Bucket
- Creates an S3 bucket for object storage
- Used by the web application to store and retrieve files

---

## Importing SSL Certificate Using AWS CLI

SSL certificates are required for secure HTTPS connections. You can import certificates into AWS Certificate Manager (ACM) using the AWS CLI.

### Certificate Files Required

For certificates from providers like Namecheap:
- **Certificate file:** `.crt` extension
- **Certificate chain file:** `.ca-bundle` extension
- **Private key file:** `.key` extension (generated with OpenSSL)

### Import Certificate Command
```bash
aws acm import-certificate --certificate fileb://your_certificate.crt --certificate-chain fileb://your_certificate_chain_file.ca-bundle --private-key fileb://your_private_key.key
```

> **Note:** These files can also be `.pem` files as stated in the official AWS documentation. Adjust the file extensions accordingly.

### Example with .pem Files
```bash
aws acm import-certificate --certificate fileb://certificate.pem --certificate-chain fileb://certificate_chain.pem --private-key fileb://private_key.pem
```

---

## Important Notes

- ⚠️ **Always set the AWS profile** before running Terraform commands
- 🔒 **Never commit** AWS credentials or sensitive variable files to version control
- 📋 **Use `terraform plan`** before `terraform apply` to review changes
- 💰 **Run `terraform destroy`** when done to avoid unnecessary AWS charges
- 🔐 **Keep your SSL certificate files secure** and never share private keys
- ✅ **Validate your Terraform configuration** regularly using `terraform validate`
- 📝 **Use variable files** (`.tfvars`) to manage different environments (dev, staging, prod)

---

## Troubleshooting

### Common Issues

**Issue: AWS credentials not found**
- Solution: Ensure you've run `aws configure` and set the `AWS_PROFILE` environment variable

**Issue: Terraform init fails**
- Solution: Check your internet connection and verify that the Terraform registry is accessible

**Issue: Terraform apply fails with permission errors**
- Solution: Verify that your AWS IAM user has sufficient permissions to create the required resources

**Issue: SSL certificate import fails**
- Solution: Ensure certificate files are in the correct format and the private key matches the certificate

---

## Best Practices

1. **Use version control** for your Terraform configuration files
2. **Implement remote state** storage using S3 and DynamoDB for team collaboration
3. **Use workspaces** to manage multiple environments
4. **Tag all resources** for better cost tracking and management
5. **Implement security groups** with the principle of least privilege
6. **Regular backups** of RDS instances
7. **Monitor costs** using AWS Cost Explorer

---

## Additional Resources

- [Terraform Documentation](https://www.terraform.io/docs)
- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
- [AWS Certificate Manager Documentation](https://docs.aws.amazon.com/acm/)

---

## Contributing

If you'd like to contribute to this project, please fork the repository and create a pull request with your changes.

---

## License

[Include your license information here]# tf-aws-infra
Terraform: Install Terraform on your local machine.
AWS CLI: Install and configure the AWS CLI on your system.



Set Up AWS CLI
	1.	Configure AWS CLI with aws configure.
	2.	Set up AWS credentials:
    Commands:-
        aws configure --profile <your-profile-name>

    1. Initialize Terraform
    Commands:-
        terraform init

    2. Plan Infrastructure
    Commands:-
        terraform plan -var-file=dev.tfvars

    3. Apply Configuration
        Commands:-
        terraform apply -var-file=dev.tfvars

    4. Destroy Infrastructure
    Commands:-
        terraform apply -var-file=dev.tfvars




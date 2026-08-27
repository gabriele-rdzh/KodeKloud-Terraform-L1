# Task 13: Create Private S3 Bucket Using Terraform

## Objective

As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS using Terraform. They plan to utilize both private and public S3 buckets to store the relevant data. Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.

Create an S3 bucket using Terraform with the following details:

1) The name of the S3 bucket must be `devops-s3-86`.

2) The S3 bucket must block all public access, making it a private bucket.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

Notes:

Use Terraform to provision the S3 bucket.
Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.
Ensure the resources are created in the us-east-1 region.
The bucket must have block public access enabled to restrict any public access.

## Solution
to create this bucket we'll just need `aws_s3_bucket` and `aws_s3_bucket_public_access_block`. and we're going to set the `aws_s3_bucket_public_access_block` options to `true`
```hcl
resource "aws_s3_bucket" "devops_bucket" {
    bucket = "devops-s3-86"
}

resource "aws_s3_bucket_public_access_block" "block_public" {
  bucket = aws_s3_bucket.devops_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```
Now let's create our bucket by using `init`, `plan` and `apply`
```bash
terraform init
# Output
Initializing the backend...
Initializing provider plugins...
...

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

We can always use `validate` to check for errors (optional)
```bash
terraform validate
# Output
Success! The configuration is valid.
```
let's go with `plan`
```bash
terraform plan
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.devops_bucket will be created
  + resource "aws_s3_bucket" "devops_bucket" {
      ...
      + bucket                      = "devops-s3-86"
      ...
    }

  # aws_s3_bucket_public_access_block.block_public will be created
  + resource "aws_s3_bucket_public_access_block" "block_public" {
      + block_public_acls       = true
      + block_public_policy     = true
      ...
      + ignore_public_acls      = true
      + restrict_public_buckets = true
    }

Plan: 2 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.devops_bucket will be created
  + resource "aws_s3_bucket" "devops_bucket" {
      ...
      + bucket                      = "devops-s3-86"
      ...
    }

  # aws_s3_bucket_public_access_block.block_public will be created
  + resource "aws_s3_bucket_public_access_block" "block_public" {
      + block_public_acls       = true
      + block_public_policy     = true
      ...
      + ignore_public_acls      = true
      + restrict_public_buckets = true
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_s3_bucket.devops_bucket: Creating...
aws_s3_bucket.devops_bucket: Creation complete after 0s [id=devops-s3-86]
aws_s3_bucket_public_access_block.block_public: Creating...
aws_s3_bucket_public_access_block.block_public: Creation complete after 0s [id=devops-s3-86]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

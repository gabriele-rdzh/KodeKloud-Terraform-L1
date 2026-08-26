# Task 12: Create Public S3 Bucket Using Terraform

## Objective

As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS. They plan to utilize both private and public S3 buckets to store the relevant data. Given the ongoing migration of other infrastructure to AWS, it is logical to consolidate data storage within the AWS environment as well.

Create a public S3 bucket named `datacenter-s3-11859` using Terraform.

Ensure the bucket is accessible publicly once created by setting the proper ACL.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

Notes:

Create the resources only in the us-east-1 region.
Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.
The name of the S3 bucket should be based on datacenter-s3-11859.
You can use the ACL settings to ensure the bucket is publicly accessible.

## Solution
To create an S3 bucket as requested, we'll need the following resources
```hcl
resource "aws_s3_bucket" "datacenter_bucket" {
    bucket = "datacenter-s3-11859"
}

resource "aws_s3_bucket_ownership_controls" "bucket_ownership" {
    bucket = aws_s3_bucket.datacenter_bucket.id
    rule {
        object_ownership = "BucketOwnerPreferred"
    }
}

resource "aws_s3_bucket_public_access_block" "allow_public" {
  bucket = aws_s3_bucket.datacenter_bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_acl" "bucket_acl" {
  depends_on = [
    aws_s3_bucket_ownership_controls.bucket_ownership,
    aws_s3_bucket_public_access_block.allow_public,
  ]

  bucket = aws_s3_bucket.datacenter_bucket.id
  acl    = "public-read"
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

  # aws_s3_bucket.datacenter_bucket will be created
  + resource "aws_s3_bucket" "datacenter_bucket" {
      ...
      + bucket                      = "datacenter-s3-11859"
      ...
    }

  # aws_s3_bucket_acl.bucket_acl will be created
  + resource "aws_s3_bucket_acl" "bucket_acl" {
      + acl    = "public-read"
      ...
    }

  # aws_s3_bucket_ownership_controls.bucket_ownership will be created
  + resource "aws_s3_bucket_ownership_controls" "bucket_ownership" {
      ...
      + rule {
          + object_ownership = "BucketOwnerPreferred"
        }
    }

  # aws_s3_bucket_public_access_block.allow_public will be created
  + resource "aws_s3_bucket_public_access_block" "allow_public" {
      + block_public_acls       = false
      + block_public_policy     = false
      ...
      + ignore_public_acls      = false
      + restrict_public_buckets = false
    }

Plan: 4 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply -auto-approve


Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.datacenter_bucket will be created
  + resource "aws_s3_bucket" "datacenter_bucket" {
      ...
      + bucket                      = "datacenter-s3-11859"
      ...
    }

  # aws_s3_bucket_acl.bucket_acl will be created
  + resource "aws_s3_bucket_acl" "bucket_acl" {
      + acl    = "public-read"
      ...

    }

  # aws_s3_bucket_ownership_controls.bucket_ownership will be created
  + resource "aws_s3_bucket_ownership_controls" "bucket_ownership" {
      ...

      + rule {
          + object_ownership = "BucketOwnerPreferred"
        }
    }

  # aws_s3_bucket_public_access_block.allow_public will be created
  + resource "aws_s3_bucket_public_access_block" "allow_public" {
      + block_public_acls       = false
      + block_public_policy     = false
      ...
      + ignore_public_acls      = false
      + restrict_public_buckets = false
    }

Plan: 4 to add, 0 to change, 0 to destroy.
aws_s3_bucket.datacenter_bucket: Creating...
aws_s3_bucket.datacenter_bucket: Creation complete after 1s [id=datacenter-s3-11859]
aws_s3_bucket_ownership_controls.bucket_ownership: Creating...
aws_s3_bucket_public_access_block.allow_public: Creating...
aws_s3_bucket_public_access_block.allow_public: Creation complete after 0s [id=datacenter-s3-11859]
aws_s3_bucket_ownership_controls.bucket_ownership: Creation complete after 0s [id=datacenter-s3-11859]
aws_s3_bucket_acl.bucket_acl: Creating...
aws_s3_bucket_acl.bucket_acl: Creation complete after 0s [id=datacenter-s3-11859,public-read]

Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

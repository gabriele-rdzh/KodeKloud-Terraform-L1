# Task 22: CloudFormation Template Deployment Using Terraform

## Objective

The Nautilus DevOps team is working on automating infrastructure deployment using AWS CloudFormation. As part of this effort, they need to create a CloudFormation stack that provisions an S3 bucket with versioning enabled.

Create a CloudFormation stack named `xfusion-stack` using `Terraform`. This stack should contain an S3 bucket named `xfusion-bucket-913188179` as a resource, and the bucket must have versioning enabled.

## Solution

To create a cloudformation stack, we'll use the following code. As you can see in the `template_body`, we'll define the reourses we'll need along with their properties.
```hcl
resource "aws_cloudformation_stack" "xfusion_stack" {
    name = "xfusion-stack"

    template_body = jsonencode({
        Resources = {
            myS3Bucket = {
                Type = "AWS::S3::Bucket"
                Properties = {
                    BucketName = "xfusion-bucket-913188179"
                    VersioningConfiguration = {
                        Status = "Enabled"
                    }
                }
            }
        }
    })
}
```

Now let's create our new resource by using `init`, `plan` and `apply`

```bash
terraform init
# Output
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "5.91.0"...
- Installing hashicorp/aws v5.91.0...
- Installed hashicorp/aws v5.91.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

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

  # aws_cloudformation_stack.xfusion_stack will be created
  + resource "aws_cloudformation_stack" "xfusion_stack" {
      + id            = (known after apply)
      + name          = "xfusion-stack"
      + outputs       = (known after apply)
      + parameters    = (known after apply)
      + policy_body   = (known after apply)
      + tags_all      = (known after apply)
      + template_body = jsonencode(
            {
              + Resources = {
                  + myS3Bucket = {
                      + Properties = {
                          + BucketName              = "xfusion-bucket-913188179"
                          + VersioningConfiguration = {
                              + Status = "Enabled"
                            }
                        }
                      + Type       = "AWS::S3::Bucket"
                    }
                }
            }
        )
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

And finally `apply`
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_cloudformation_stack.xfusion_stack will be created
  + resource "aws_cloudformation_stack" "xfusion_stack" {
      + id            = (known after apply)
      + name          = "xfusion-stack"
      + outputs       = (known after apply)
      + parameters    = (known after apply)
      + policy_body   = (known after apply)
      + tags_all      = (known after apply)
      + template_body = jsonencode(
            {
              + Resources = {
                  + myS3Bucket = {
                      + Properties = {
                          + BucketName              = "xfusion-bucket-913188179"
                          + VersioningConfiguration = {
                              + Status = "Enabled"
                            }
                        }
                      + Type       = "AWS::S3::Bucket"
                    }
                }
            }
        )
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_cloudformation_stack.xfusion_stack: Creating...
aws_cloudformation_stack.xfusion_stack: Still creating... [10s elapsed]
aws_cloudformation_stack.xfusion_stack: Creation complete after 10s 

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

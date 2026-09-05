# Task 17: Create DynamoDB Table Using Terraform

## Objective

The Nautilus DevOps team needs to set up a DynamoDB table for storing user data. They need to create a DynamoDB table with the following specifications:

1) The table name should be `datacenter-users`.

2) The primary key should be `datacenter_id` (String).

3) The table should use `PAY_PER_REQUEST` billing mode.

Use Terraform to create this DynamoDB table. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to create the DynamoDB table.

## Solution

To create the table, we'll use the following code. You'll notice two things: to refer to the primary key, we'll use `hash_key`, and for the attributes, we'll use `type = "S"` instead of specifying `String`
```hcl
resource "aws_dynamodb_table" "datacenter_user" {
    name         = "datacenter-users"
    billing_mode = "PAY_PER_REQUEST"
    hash_key     = "datacenter_id"

    attribute {
        name = "datacenter_id"
        type = "S"
    }
}
```

Now let's create our DynamoDB table by using `init`, `plan` and `apply`
```bash
terraform init
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
Success! The configuration is valid.
```

let's go with `plan`
```bash
terraform plan

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_dynamodb_table.datacenter_user will be created
  + resource "aws_dynamodb_table" "datacenter_user" {
      + arn              = (known after apply)
      + billing_mode     = "PAY_PER_REQUEST"
      + hash_key         = "datacenter_id"
      + id               = (known after apply)
      + name             = "datacenter-users"
      + read_capacity    = (known after apply)
      + stream_arn       = (known after apply)
      + stream_label     = (known after apply)
      + stream_view_type = (known after apply)
      + tags_all         = (known after apply)
      + write_capacity   = (known after apply)

      + attribute {
          + name = "datacenter_id"
          + type = "S"
        }

      + point_in_time_recovery (known after apply)

      + server_side_encryption (known after apply)

      + ttl (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

And finally `apply`
```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_dynamodb_table.datacenter_user will be created
  + resource "aws_dynamodb_table" "datacenter_user" {
      + arn              = (known after apply)
      + billing_mode     = "PAY_PER_REQUEST"
      + hash_key         = "datacenter_id"
      + id               = (known after apply)
      + name             = "datacenter-users"
      + read_capacity    = (known after apply)
      + stream_arn       = (known after apply)
      + stream_label     = (known after apply)
      + stream_view_type = (known after apply)
      + tags_all         = (known after apply)
      + write_capacity   = (known after apply)

      + attribute {
          + name = "datacenter_id"
          + type = "S"
        }

      + point_in_time_recovery (known after apply)

      + server_side_encryption (known after apply)

      + ttl (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_dynamodb_table.datacenter_user: Creating...
aws_dynamodb_table.datacenter_user: Creation complete after 2s [id=datacenter-users]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

# Task 21: CloudWatch Setup Using Terraform

## Objective

The Nautilus DevOps team needs to set up CloudWatch logging for their application. They need to create a CloudWatch log group and log stream with the following specifications:

1) The log group name should be `nautilus-log-group`.

2) The log stream name should be `nautilus-log-stream`.

## Solution

To create the following resources, we'll just need to "call" the `log_group` name in the `log_stream`
```hcl
resource "aws_cloudwatch_log_group" "nautilus_log_group" {
    name = "nautilus-log-group"
}

resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
    name           = "nautilus-log-stream"
    log_group_name = aws_cloudwatch_log_group.nautilus_log_group.name
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

  # aws_cloudwatch_log_group.nautilus_log_group will be created
  + resource "aws_cloudwatch_log_group" "nautilus_log_group" {
      + arn               = (known after apply)
      + id                = (known after apply)
      + log_group_class   = (known after apply)
      + name              = "nautilus-log-group"
      + name_prefix       = (known after apply)
      + retention_in_days = 0
      + skip_destroy      = false
      + tags_all          = (known after apply)
    }

  # aws_cloudwatch_log_stream.nautilus_log_stream will be created
  + resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
      + arn            = (known after apply)
      + id             = (known after apply)
      + log_group_name = "nautilus-log-group"
      + name           = "nautilus-log-stream"
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

  # aws_cloudwatch_log_group.nautilus_log_group will be created
  + resource "aws_cloudwatch_log_group" "nautilus_log_group" {
      + arn               = (known after apply)
      + id                = (known after apply)
      + log_group_class   = (known after apply)
      + name              = "nautilus-log-group"
      + name_prefix       = (known after apply)
      + retention_in_days = 0
      + skip_destroy      = false
      + tags_all          = (known after apply)
    }

  # aws_cloudwatch_log_stream.nautilus_log_stream will be created
  + resource "aws_cloudwatch_log_stream" "nautilus_log_stream" {
      + arn            = (known after apply)
      + id             = (known after apply)
      + log_group_name = "nautilus-log-group"
      + name           = "nautilus-log-stream"
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_cloudwatch_log_group.nautilus_log_group: Creating...
aws_cloudwatch_log_group.nautilus_log_group: Creation complete after 0s [id=nautilus-log-group]
aws_cloudwatch_log_stream.nautilus_log_stream: Creating...
aws_cloudwatch_log_stream.nautilus_log_stream: Creation complete after 0s [id=nautilus-log-stream]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

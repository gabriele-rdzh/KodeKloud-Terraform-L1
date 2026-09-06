# Task 18: Create Kinesis Stream Using Terraform

## Objective

The Nautilus DevOps team needs to create an AWS Kinesis data stream for real-time data processing. This stream will be used to ingest and process large volumes of streaming data, which will then be consumed by various applications for analytics and real-time decision-making.

1. The stream should be named `nautilus-stream`.

2. Use `Terraform` to create this Kinesis stream.

## Solution

In Terraform, to create a Kinesis Stream, we'll need the following code... and, in fact the only required field is the name.
```hcl
resource "aws_kinesis_stream" "nautilus_stream" {
  name             = "nautilus-stream"
  shard_count      = 1
  retention_period = 24
}
```

Now let's create our Kinesis stream by using `init`, `plan` and `apply`
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

  # aws_kinesis_stream.nautilus_stream will be created
  + resource "aws_kinesis_stream" "nautilus_stream" {
      + arn                       = (known after apply)
      + encryption_type           = "NONE"
      + enforce_consumer_deletion = false
      + id                        = (known after apply)
      + name                      = "nautilus-stream"
      + retention_period          = 24
      + shard_count               = 1
      + tags_all                  = (known after apply)

      + stream_mode_details (known after apply)
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

  # aws_kinesis_stream.nautilus_stream will be created
  + resource "aws_kinesis_stream" "nautilus_stream" {
      + arn                       = (known after apply)
      + encryption_type           = "NONE"
      + enforce_consumer_deletion = false
      + id                        = (known after apply)
      + name                      = "nautilus-stream"
      + retention_period          = 24
      + shard_count               = 1
      + tags_all                  = (known after apply)

      + stream_mode_details (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_kinesis_stream.nautilus_stream: Creating...
aws_kinesis_stream.nautilus_stream: Still creating... [10s elapsed]
aws_kinesis_stream.nautilus_stream: Still creating... [20s elapsed]
aws_kinesis_stream.nautilus_stream: Creation complete after 20s [id=arn:aws:kinesis:us-east-1:000000000000:stream/nautilus-stream]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_kinesis_stream.nautilus_stream will be created
  + resource "aws_kinesis_stream" "nautilus_stream" {
      + arn                       = (known after apply)
      + encryption_type           = "NONE"
      + enforce_consumer_deletion = false
      + id                        = (known after apply)
      + name                      = "nautilus-stream"
      + retention_period          = 24
      + shard_count               = 1
      + tags_all                  = (known after apply)

      + stream_mode_details (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_kinesis_stream.nautilus_stream: Creating...
aws_kinesis_stream.nautilus_stream: Still creating... [10s elapsed]
aws_kinesis_stream.nautilus_stream: Still creating... [20s elapsed]
aws_kinesis_stream.nautilus_stream: Creation complete after 20s [id=arn:aws:kinesis:us-east-1:000000000000:stream/nautilus-stream]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

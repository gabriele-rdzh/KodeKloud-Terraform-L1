# Task 09: Create EBS Volume Using Terraform

## Objective

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create an AWS EBS volume using Terraform with the following requirements:

- Name of the volume should be `devops-volume`.

- Volume type must be `gp3`.

- Volume size must be `2 GiB`.

- Ensure the volume is created in `us-east-1`.

## Solution
To create the EBS volume, we'll write the following `main.tf`; we'll only need `availability_zone`, `type` and `size`

```hcl
resource "aws_ebs_volume" "devops_volume" {
    availability_zone = "us-east-1a"
    size              = 2
    type              = "gp3"

    tags = {
        Name = "devops-volume"
    }
}
```


Now let's create our volume by using `init`, `plan` and `apply`
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

  # aws_ebs_volume.devops_volume will be created
  + resource "aws_ebs_volume" "devops_volume" {
      + arn               = (known after apply)
      + availability_zone = "us-east-1a"
      ...
      + size              = 2
      + snapshot_id       = (known after apply)
      + tags              = {
          + "Name" = "devops-volume"
        }
      + tags_all          = {
          + "Name" = "devops-volume"
        }
      + throughput        = (known after apply)
      + type              = "gp3"
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

  # aws_ebs_volume.devops_volume will be created
  + resource "aws_ebs_volume" "devops_volume" {
      + arn               = (known after apply)
      + availability_zone = "us-east-1a"
      ...
      + size              = 2
      + snapshot_id       = (known after apply)
      + tags              = {
          + "Name" = "devops-volume"
        }
      + tags_all          = {
          + "Name" = "devops-volume"
        }
      + throughput        = (known after apply)
      + type              = "gp3"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_ebs_volume.devops_volume: Creating...
aws_ebs_volume.devops_volume: Still creating... [10s elapsed]
aws_ebs_volume.devops_volume: Creation complete after 11s [id=vol-135c58be271727ef6]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

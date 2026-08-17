# Task 06: Create Elastic IP Using Terraform

## Objective

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, allocate an `Elastic IP` address named `nautilus-eip` using Terraform.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

## Solution

For this task, we're going to write the `m̀ain.tf` file as follows.
```hcl
resource "aws_eip" "nautilus_eip" {
    domain = "vpc"

    tags = {
        Name = "nautilus-eip"
    }
}
```
Now let's allocate the `Elastic IP` using `init`, `plan` and `apply`
```bash
terraform init
# Output
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

  # aws_eip.nautilus_eip will be created
  + resource "aws_eip" "nautilus_eip" {
      + allocation_id        = (known after apply)
      + arn                  = (known after apply)
      + association_id       = (known after apply)
      + carrier_ip           = (known after apply)
      + customer_owned_ip    = (known after apply)
      + domain               = "vpc"
      + id                   = (known after apply)
      + instance             = (known after apply)
      + ipam_pool_id         = (known after apply)
      + network_border_group = (known after apply)
      + network_interface    = (known after apply)
      + private_dns          = (known after apply)
      + private_ip           = (known after apply)
      + ptr_record           = (known after apply)
      + public_dns           = (known after apply)
      + public_ip            = (known after apply)
      + public_ipv4_pool     = (known after apply)
      + tags                 = {
          + "Name" = "nautilus-eip"
        }
      + tags_all             = {
          + "Name" = "nautilus-eip"
        }
      + vpc                  = (known after apply)
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

  # aws_eip.nautilus_eip will be created
  + resource "aws_eip" "nautilus_eip" {
      + allocation_id        = (known after apply)
      + arn                  = (known after apply)
      + association_id       = (known after apply)
      + carrier_ip           = (known after apply)
      + customer_owned_ip    = (known after apply)
      + domain               = "vpc"
      + id                   = (known after apply)
      + instance             = (known after apply)
      + ipam_pool_id         = (known after apply)
      + network_border_group = (known after apply)
      + network_interface    = (known after apply)
      + private_dns          = (known after apply)
      + private_ip           = (known after apply)
      + ptr_record           = (known after apply)
      + public_dns           = (known after apply)
      + public_ip            = (known after apply)
      + public_ipv4_pool     = (known after apply)
      + tags                 = {
          + "Name" = "nautilus-eip"
        }
      + tags_all             = {
          + "Name" = "nautilus-eip"
        }
      + vpc                  = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_eip.nautilus_eip: Creating...
aws_eip.nautilus_eip: Creation complete after 0s [id=eipalloc-eb3b423789bb086f5]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

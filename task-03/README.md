# Task 03: Create VPC Using Terraform

## Objective

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a VPC named `nautilus-vpc` in region `us-east-1` with any `IPv4` CIDR block through terraform.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

## Solution

First, we create the `main.tf` file in the specified path and add the VPC resource configuration.

```bash
cd /home/bob/terraform
vi main.tf
```

```hcl
resource "aws_vpc" "nautilus_vpc" {
    cidr_block       = "10.0.0.0/16"
    instance_tenancy = "default"

    tags = {
        Name = "nautilus-vpc"
    }
}
```

Now that we've created the `main.tf` file, we can initialize terraform with `terraform init`
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
We can validate the file(optional)
```bash
terraform validate
# Output
Success! The configuration is valid.
```
Now that everything is set up cerrectly, let's run `terraform plan`
```bash
terraform plan
# Output
Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.nautilus_vpc will be created
  + resource "aws_vpc" "nautilus_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "10.0.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = (known after apply)
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags                                 = {
          + "Name" = "nautilus-vpc"
        }
      + tags_all                             = {
          + "Name" = "nautilus-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
Finally, we use `terraform apply` to create the resources—in this case, the VPC
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.nautilus_vpc will be created
  + resource "aws_vpc" "nautilus_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "10.0.0.0/16"
      + default_network_acl_id               = (known after apply)
      + default_route_table_id               = (known after apply)
      + default_security_group_id            = (known after apply)
      + dhcp_options_id                      = (known after apply)
      + enable_dns_hostnames                 = (known after apply)
      + enable_dns_support                   = true
      + enable_network_address_usage_metrics = (known after apply)
      + id                                   = (known after apply)
      + instance_tenancy                     = "default"
      + ipv6_association_id                  = (known after apply)
      + ipv6_cidr_block                      = (known after apply)
      + ipv6_cidr_block_network_border_group = (known after apply)
      + main_route_table_id                  = (known after apply)
      + owner_id                             = (known after apply)
      + tags                                 = {
          + "Name" = "nautilus-vpc"
        }
      + tags_all                             = {
          + "Name" = "nautilus-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_vpc.nautilus_vpc: Creating...
aws_vpc.nautilus_vpc: Creation complete after 1s [id=vpc-4edaa05bca5902e6b]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

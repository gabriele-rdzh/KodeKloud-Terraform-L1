# Task 05: Create VPC with IPv6 Using Terraform

## Objective

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

For this task, create a VPC named `datacenter-vpc` in the `us-east-1` region with the Amazon-provided IPv6 CIDR block using terraform.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

## Solution

For this task, when writing the `main.tf` file, we'll add `assign_generated_ipv6_cidr_block = true` to meet the objective.
```hcl
resource "aws_vpc" "datacenter-vpc" {
    cidr_block                       = "10.0.0.0/16"
    assign_generated_ipv6_cidr_block = true

    tags = {
        Name = "datacenter-vpc"
    }
}
```
Now let's create the VPC using `init`, `plan` and `apply`
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

  # aws_vpc.datacenter-vpc will be created
  + resource "aws_vpc" "datacenter-vpc" {
      + arn                                  = (known after apply)
      + assign_generated_ipv6_cidr_block     = true
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
          + "Name" = "datacenter-vpc"
        }
      + tags_all                             = {
          + "Name" = "datacenter-vpc"
        }
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

  # aws_vpc.datacenter-vpc will be created
  + resource "aws_vpc" "datacenter-vpc" {
      + arn                                  = (known after apply)
      + assign_generated_ipv6_cidr_block     = true
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
          + "Name" = "datacenter-vpc"
        }
      + tags_all                             = {
          + "Name" = "datacenter-vpc"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_vpc.datacenter-vpc: Creating...
aws_vpc.datacenter-vpc: Still creating... [10s elapsed]
aws_vpc.datacenter-vpc: Creation complete after 11s [id=vpc-0c54544721f90acbe]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

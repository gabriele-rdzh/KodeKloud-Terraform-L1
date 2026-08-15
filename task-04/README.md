# Task 04: Create VPC with CIDR Using Terraform

## Objective

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

Create a VPC named `nautilus-vpc` in `us-east-1` region with `192.168.0.0/24` IPv4 CIDR using terraform.


The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

## Solution

Let's create the `main.tf` file with the requiered configuration
```hcl
resource "aws_vpc" "nautilus_vpc" {
    cidr_block = "192.168.0.0/24"

    tags = {
        Name = "nautilus-vpc"
    }
}
```
Now that `main.tf` has been created, we just need to use `init`, `plan`, and `apply`, and we're done.
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
We can also use `validate` to make sure there are no errors in `main.tf`
```bash
terraform validate
# Output
Success! The configuration is valid.
```

```bash
terraform plan
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.nautilus_vpc will be created
  + resource "aws_vpc" "nautilus_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "192.168.0.0/24"
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

```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_vpc.nautilus_vpc will be created
  + resource "aws_vpc" "nautilus_vpc" {
      + arn                                  = (known after apply)
      + cidr_block                           = "192.168.0.0/24"
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
aws_vpc.nautilus_vpc: Creation complete after 0s [id=vpc-dddd303641483ba3c]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

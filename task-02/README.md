# Task 02: Create Security Group Using Terraform

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

## Objective

Use *Terraform* to create a security group under the default VPC with the following requirements:

1) The name of the security group must be `devops-sg`.

2) The description must be `Security group for Nautilus App Servers`.

3) Add an inbound rule of type `HTTP`, with a port range of `80`, and source CIDR range `0.0.0.0/0`.

4) Add another inbound rule of type `SSH`, with a port range of `22`, and source CIDR range  `0.0.0.0/0`.

Ensure that the security group is created in the us-east-1 region using Terraform. The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

## Solution

Create `main.tf` file

```bash
cd /home/bob/terraform
vi main.tf
```
```hcl
resource "Security group" "devops-sg" {
    name        = "devops-sg"
    description = "Security group for Nautilus App Servers"

    #Inbound rule for http
    ingress {
        from_port   = 80
        to_port     = 80
        protocol    = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }

    #Inbound rule for ssh
    ingress {
        from_port   = 22
        to_port     = 22
        protocol    = "ssh"
        cidr_blocks = ["0.0.0.0/0"]
    }
}
```
Use `terraform init` to initialize terraform 

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
Now we can use `terraform plan` to validate the resources that will be created

```bash
terraform plan
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_security_group.devops_sg will be created
  + resource "aws_security_group" "devops_sg" {
      + arn                    = (known after apply)
      + description            = "Security group for Nautilus App Servers"
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 22
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "ssh"
              + security_groups  = []
              + self             = false
              + to_port          = 22
                # (1 unchanged attribute hidden)
            },
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 80
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 80
                # (1 unchanged attribute hidden)
            },
        ]
      + name                   = "devops-sg"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags_all               = (known after apply)
      + vpc_id                 = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
After validating the resources to be created. we use `terraform apply` to create them

```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_security_group.devops_sg will be created
  + resource "aws_security_group" "devops_sg" {
      + arn                    = (known after apply)
      + description            = "Security group for Nautilus App Servers"
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = [
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 22
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "ssh"
              + security_groups  = []
              + self             = false
              + to_port          = 22
                # (1 unchanged attribute hidden)
            },
          + {
              + cidr_blocks      = [
                  + "0.0.0.0/0",
                ]
              + from_port        = 80
              + ipv6_cidr_blocks = []
              + prefix_list_ids  = []
              + protocol         = "tcp"
              + security_groups  = []
              + self             = false
              + to_port          = 80
                # (1 unchanged attribute hidden)
            },
        ]
      + name                   = "devops-sg"
      + name_prefix            = (known after apply)
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags_all               = (known after apply)
      + vpc_id                 = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_security_group.devops_sg: Creating...
aws_security_group.devops_sg: Creation complete after 2s [id=sg-174ebfcd0a33e66bf]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```
## Key Learnings

We can use `terraform validate` to check the syntax of the code. It's useful for detecting errors before running `terraform plan` or `terraform apply`

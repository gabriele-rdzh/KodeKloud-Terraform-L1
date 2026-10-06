# Task 25: Change Instance Type Using Terraform

## Objective

During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the `Status check` is completed (if it's still in `Initializing` state) before making any changes to the instance.

Change the instance type from `t2.micro` to `t2.nano` for `devops-ec2` instance using `terraform`.

Make sure the EC2 instance devops-ec2 is in running state after the change.

## Solution
to do this, we just need to chance the instance type(for now). 

Before
```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = ""
  vpc_security_group_ids = [
    "sg-d80a5b2c17e203e27"
  ]

  tags = {
    Name = "devops-ec2"
  }
}
```
After
```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.nano"
  subnet_id     = ""
  vpc_security_group_ids = [
    "sg-d80a5b2c17e203e27"
  ]

  tags = {
    Name = "devops-ec2"
  }
}
```
and we moved forward as usual with `ìnit`, `plan` and `apply`
```bash
terraform init
# Output
Initializing the backend...
Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Using previously-installed hashicorp/aws v5.91.0

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

let's go with `plan`. Here we can already see that the instance type is going to change.
```bash
terraform plan
# Output
aws_instance.ec2: Refreshing state... [id=i-574351012d37016f3]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.ec2 will be updated in-place
  ~ resource "aws_instance" "ec2" {
        id                                   = "i-574351012d37016f3"
      ~ instance_type                        = "t2.micro" -> "t2.nano"
      ~ public_dns                           = "ec2-54-214-98-15.compute-1.amazonaws.com" -> (known after apply)
      ~ public_ip                            = "54.214.98.15" -> (known after apply)
        tags                                 = {
            "Name" = "devops-ec2"
        }
        # (34 unchanged attributes hidden)

        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```
And finally `apply`. this takes a long time. Please be patient
```bash
terraform apply
# Output
aws_instance.ec2: Refreshing state... [id=i-574351012d37016f3]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_instance.ec2 will be updated in-place
  ~ resource "aws_instance" "ec2" {
        id                                   = "i-574351012d37016f3"
      ~ instance_type                        = "t2.micro" -> "t2.nano"
      ~ public_dns                           = "ec2-54-214-98-15.compute-1.amazonaws.com" -> (known after apply)
      ~ public_ip                            = "54.214.98.15" -> (known after apply)
        tags                                 = {
            "Name" = "devops-ec2"
        }
        # (34 unchanged attributes hidden)

        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.ec2: Modifying... [id=i-574351012d37016f3]
aws_instance.ec2: Still modifying... [id=i-574351012d37016f3, 10s elapsed]
aws_instance.ec2: Still modifying... [id=i-574351012d37016f3, 20s elapsed]
aws_instance.ec2: Modifications complete after 20s [id=i-574351012d37016f3]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

Here we can see that the instance is already running
```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=devops-ec2" --query "Reservations[*].Instances[*].[InstanceId,State.Name]" --output table
# Output
------------------------------------
|         DescribeInstances        |
+----------------------+-----------+
|  i-574351012d37016f3 |  running  |
+----------------------+-----------+
```

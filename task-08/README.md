# Task 08: Create AMI Using Terraform

## Objective

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

1. For this task, create an AMI from an existing EC2 instance named `xfusion-ec2` using Terraform.

2. Name of the AMI should be `xfusion-ec2-ami`, make sure AMI is in `available` state.

3. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate `.tf` file) to create the AMI.

## Solution

For this task, we need tu update `main.tf` by adding the `aws_ami_from_instance` resource and referencing the instance using `source_instance_id`

```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-42af040f6c8b411e0"
  ]

  tags = {
    Name = "xfusion-ec2"
  }
}

resource "aws_ami_from_instance" "xfusion_ec2_ami" {
  name               = "xfusion-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```
Now let's create our AMI by using `init`, `plan` and `apply`
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
aws_instance.ec2: Refreshing state... [id=i-39c7ee0ad62c2f520]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ami_from_instance.xfusion_ec2_ami will be created
  + resource "aws_ami_from_instance" "xfusion_ec2_ami" {
      ...
      + name                 = "xfusion-ec2-ami"
      ...
      + source_instance_id   = "i-39c7ee0ad62c2f520"
      ...
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply
# Output
aws_instance.ec2: Refreshing state... [id=i-39c7ee0ad62c2f520]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ami_from_instance.xfusion_ec2_ami will be created
  + resource "aws_ami_from_instance" "xfusion_ec2_ami" {
      ...
      + name                 = "xfusion-ec2-ami"
      ...
      + source_instance_id   = "i-39c7ee0ad62c2f520"
      ...
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_ami_from_instance.xfusion_ec2_ami: Creating...
aws_ami_from_instance.xfusion_ec2_ami: Creation complete after 5s [id=ami-763ceb280cd51b51b]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

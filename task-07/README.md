# Task 07: Create EC2 Instance Using Terraform

## Objective

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using `Terraform` with the following requirements:

The EC2 instance must use the value `devops-ec2` as its Name tag, which defines the instance name in AWS.

Use the `Amazon Linux` `ami-0c101f26f147fa7fd` to launch this instance.

The Instance type must be `t2.micro`.

Create a new RSA key named `devops-kp`.

Attach the default (available by default) security group.

## Solution

For this task, we're going to define three resources within out `main.tf` file, the ones needed for the `key pair` and the instance.
```hcl
# generates the RSA private key
resource "tls_private_key" "key" {
    algorithm = "RSA"
    rsa_bits  = 2048
}

# create the Key Pair in AWS
resource "aws_key_pair" "devops_kp" {
    key_name   = "devops-kp"
    public_key = tls_private_key.key.public_key_openssh
}

# create the instance EC2
resource "aws_instance" "devops_ec2" {
    ami           = "ami-0c101f26f147fa7fd"
    instance_type = "t2.micro"
    key_name      = aws_key_pair.devops_kp.key_name

    tags = {
        Name = "devops-ec2"
    }
}
```
Now let's create our instace by using `init`, `plan` and `apply`
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

  # aws_instance.devops_ec2 will be created
  + resource "aws_instance" "devops_ec2" {
      + ami                                  = "ami-0c101f26f147fa7fd"
      ...
      + key_name                             = "devops-kp"
      ...
      + tags                                 = {
          + "Name" = "devops-ec2"
        }
      + tags_all                             = {
          + "Name" = "devops-ec2"
        }
      ...
    }

  # aws_key_pair.devops_kp will be created
  + resource "aws_key_pair" "devops_kp" {
      ...
      + key_name        = "devops-kp"
      ...
    }

  # tls_private_key.key will be created
  + resource "tls_private_key" "key" {
      + algorithm                     = "RSA"
      ...
      + rsa_bits                      = 2048
    }

Plan: 3 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.devops_ec2 will be created
  + resource "aws_instance" "devops_ec2" {
      + ami                                  = "ami-0c101f26f147fa7fd"
      ...
      + instance_type                        = "t2.micro"
      ...
      + key_name                             = "devops-kp"
      ...
      + tags                                 = {
          + "Name" = "devops-ec2"
        }
      + tags_all                             = {
          + "Name" = "devops-ec2"
        }
      ...
    }

  # aws_key_pair.devops_kp will be created
  + resource "aws_key_pair" "devops_kp" {
      ...
      + key_name        = "devops-kp"
      ...
    }

  # tls_private_key.key will be created
  + resource "tls_private_key" "key" {
      + algorithm                     = "RSA"
      ...
      + rsa_bits                      = 2048
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

tls_private_key.key: Creating...
tls_private_key.key: Creation complete after 1s [id=13d010d04001e2b4fba2599e33e83ed3670029cb]
aws_key_pair.devops_kp: Creating...
aws_key_pair.devops_kp: Creation complete after 1s [id=devops-kp]
aws_instance.devops_ec2: Creating...
aws_instance.devops_ec2: Still creating... [10s elapsed]
aws_instance.devops_ec2: Creation complete after 10s [id=i-4a48bacdd2dbae6d6]

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```

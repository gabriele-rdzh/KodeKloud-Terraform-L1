# Task 16: Create IAM Policy Using Terraform

## Objective

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements.

Create an IAM policy named `iampolicy_siva` in `us-east-1` region using Terraform. It must allow read-only access to the EC2 console, i.e., this policy must allow users to view all instances, AMIs, and snapshots in the Amazon EC2 console.

## Solution

This is where things start to get a little complicated, since we're going to wirte the policy they're asking for inside `main.tf`
```hcl
resource "aws_iam_policy" "iampolicy_siva" {
    name        = "iampolicy_siva"
    path        = "/"
    description = "only read policy"

    policy = jsonencode({
        Version = "2012-10-17"
        Statement = [
            {
                Effect = "Allow"
                Action = [
                    "ec2:Descibe*"
                ]
                Resource = "*"
            }
        ]
    })
}
```

Now let's create our IAM policy by using `init`, `plan` and `apply`

```bash
terraform init
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
Success! The configuration is valid.
```

let's go with `plan`

```bash
terraform plan

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_iam_policy.iampolicy-siva will be created
  + resource "aws_iam_policy" "iampolicy-siva" {
      + arn              = (known after apply)
      + attachment_count = (known after apply)
      + description      = "read only policy"
      + id               = (known after apply)
      + name             = "iampolicy_siva"
      + name_prefix      = (known after apply)
      + path             = "/"
      + policy           = jsonencode(
            {
              + Statement = [
                  + {
                      + Action   = [
                          + "ec2:Descibe*",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + policy_id        = (known after apply)
      + tags_all         = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

And finally `apply`

```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_iam_policy.iampolicy-siva will be created
  + resource "aws_iam_policy" "iampolicy-siva" {
      + arn              = (known after apply)
      + attachment_count = (known after apply)
      + description      = "read only policy"
      + id               = (known after apply)
      + name             = "iampolicy_siva"
      + name_prefix      = (known after apply)
      + path             = "/"
      + policy           = jsonencode(
            {
              + Statement = [
                  + {
                      + Action   = [
                          + "ec2:Descibe*",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + policy_id        = (known after apply)
      + tags_all         = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_iam_policy.iampolicy-siva: Creating...
aws_iam_policy.iampolicy-siva: Creation complete after 0s [id=arn:aws:iam::000000000000:policy/iampolicy_siva]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```


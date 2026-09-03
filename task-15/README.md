# Task 15: Create IAM Group Using Terraform

## Objective

The siva DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

Create an IAM group named `iamgroup_siva` using `terraform`.

## Solution

just like creating a user, creating a group is just as easy; the "complicated" part comes with the policies
```hcl
resource "aws_iam_group" "iamgroup_siva" {
    name = "iamgroup_siva"
}
```

Now let's create our IAM group by using `init`, `plan` and `apply`
```bash
terraform init
# Output
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
# Output
Success! The configuration is valid.
```

let's go with `plan`
```bash
terraform plan
# Output
Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_iam_group.iamgroup_siva will be created
  + resource "aws_iam_group" "iamgroup_siva" {
      + arn       = (known after apply)
      + id        = (known after apply)
      + name      = "iamgroup_siva"
      + path      = "/"
      + unique_id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

And finally `apply`
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_iam_group.iamgroup_siva will be created
  + resource "aws_iam_group" "iamgroup_siva" {
      + arn       = (known after apply)
      + id        = (known after apply)
      + name      = "iamgroup_siva"
      + path      = "/"
      + unique_id = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_iam_group.iamgroup_siva: Creating...
aws_iam_group.iamgroup_siva: Creation complete after 0s [id=iamgroup_siva]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

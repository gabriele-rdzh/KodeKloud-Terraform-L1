# Task 24: Secrets Manager Setup Using Terraform

## Objective

The Nautilus DevOps team needs to store sensitive data securely using AWS Secrets Manager. They need to create a secret with the following specifications:

1) The secret name should be `devops-secret`.

2) The secret value should contain a key-value pair with `username: admin` and `password: Namin123`.

3) Use `Terraform` to create the secret in AWS Secrets Manager.

## Solution
For this lab, we'll need not only the `aws_secretsmanager_secret` but also the `aws_secretsmanager_secret_version`, which is where we'll store the `username` and `password`
```hcl
resource "aws_secretsmanager_secret" "devops_secret" {
  name = "devops-secret"
}

resource "aws_secretsmanager_secret_version" "devops_secret_version" {
    secret_id = aws_secretsmanager_secret.devops_secret.id
    secret_string = jsonencode({
        username = "admin"
        password = "Namin123"
    })
}
```
Now let's create our new resource by using `init`, `plan` and `apply`

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
I owe you the plan.
You'll have to imagine the plan step.
I forgot to copy it :B

And finally `apply`. this takes a long time. Please be patient
```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_secretsmanager_secret.devops_secret will be created
  + resource "aws_secretsmanager_secret" "devops_secret" {
      ...
      + name                           = "devops-secret"
      ...
    }

  # aws_secretsmanager_secret_version.devops_secret_version will be created
  + resource "aws_secretsmanager_secret_version" "devops_secret_version" {
      ...
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

...

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

```

# Task 20: Create SSM Parameter Using Terraform

## Objective

The Nautilus DevOps team needs to create an SSM parameter in AWS with the following requirements:

1) The name of the parameter should be `devops-ssm-parameter`.

2) Set the parameter type to `String`.

3) Set the parameter value to `devops-value`.

4) The parameter should be created in the `us-east-1` region.

5) Ensure the parameter is successfully created using terraform and can be retrieved when the task is completed.

## Solution

Well, to create the resource, we'll need the following code.
```hcl
resource "aws_ssm_parameter" "devops_ssm_parameter" {
    name  = "devops-ssm-parameter"
    type  = "String"
    value = "devops-value"
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
let's go with `plan`
```bash
terraform plan

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ssm_parameter.devops_ssm_parameter will be created
  + resource "aws_ssm_parameter" "devops_ssm_parameter" {
      + arn            = (known after apply)
      + data_type      = (known after apply)
      + has_value_wo   = (known after apply)
      + id             = (known after apply)
      + insecure_value = (known after apply)
      + key_id         = (known after apply)
      + name           = "devops-ssm-parameter"
      + tags_all       = (known after apply)
      + tier           = (known after apply)
      + type           = "String"
      + value          = (sensitive value)
      + value_wo       = (write-only attribute)
      + version        = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

```
And finally `apply
```bash
terraform apply

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ssm_parameter.devops_ssm_parameter will be created
  + resource "aws_ssm_parameter" "devops_ssm_parameter" {
      + arn            = (known after apply)
      + data_type      = (known after apply)
      + has_value_wo   = (known after apply)
      + id             = (known after apply)
      + insecure_value = (known after apply)
      + key_id         = (known after apply)
      + name           = "devops-ssm-parameter"
      + tags_all       = (known after apply)
      + tier           = (known after apply)
      + type           = "String"
      + value          = (sensitive value)
      + value_wo       = (write-only attribute)
      + version        = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes 

aws_ssm_parameter.devops_ssm_parameter: Creating...
aws_ssm_parameter.devops_ssm_parameter: Creation complete after 1s [id=devops-ssm-parameter]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

```

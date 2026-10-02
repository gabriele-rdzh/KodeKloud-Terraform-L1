# Task 23: OpenSearch Setup Using Terraform

## Objective

The Nautilus DevOps team needs to set up an Amazon OpenSearch Service domain to store and search their application logs. The domain should have the following specification:

1) The domain name should be `nautilus-es`.

## Solution

Well, since the only thing we need is the domain name, here's the code we'll need
```hcl
resource "aws_opensearch_domain" "nautilus_es" {
    domain_name = "nautilus-es"
}
```

Now let's create our new resource by using `init`, `plan` and `apply`

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
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_opensearch_domain.nautilus_es will be created
  + resource "aws_opensearch_domain" "nautilus_es" {
      + access_policies                   = (known after apply)
      + advanced_options                  = (known after apply)
      + arn                               = (known after apply)
      + dashboard_endpoint                = (known after apply)
      + dashboard_endpoint_v2             = (known after apply)
      + domain_endpoint_v2_hosted_zone_id = (known after apply)
      + domain_id                         = (known after apply)
      + domain_name                       = "nautilus-es"
      + endpoint                          = (known after apply)
      + endpoint_v2                       = (known after apply)
      + engine_version                    = (known after apply)
      + id                                = (known after apply)
      + ip_address_type                   = (known after apply)
      + kibana_endpoint                   = (known after apply)
      + tags_all                          = (known after apply)

      + advanced_security_options (known after apply)

      + auto_tune_options (known after apply)

      + cluster_config (known after apply)

      + domain_endpoint_options (known after apply)

      + ebs_options (known after apply)

      + encrypt_at_rest (known after apply)

      + node_to_node_encryption (known after apply)

      + off_peak_window_options (known after apply)

      + software_update_options (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
And finally `apply`. this takes a long time. Please be patient
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_opensearch_domain.nautilus_es will be created
  + resource "aws_opensearch_domain" "nautilus_es" {
      + access_policies                   = (known after apply)
      + advanced_options                  = (known after apply)
      + arn                               = (known after apply)
      + dashboard_endpoint                = (known after apply)
      + dashboard_endpoint_v2             = (known after apply)
      + domain_endpoint_v2_hosted_zone_id = (known after apply)
      + domain_id                         = (known after apply)
      + domain_name                       = "nautilus-es"
      + endpoint                          = (known after apply)
      + endpoint_v2                       = (known after apply)
      + engine_version                    = (known after apply)
      + id                                = (known after apply)
      + ip_address_type                   = (known after apply)
      + kibana_endpoint                   = (known after apply)
      + tags_all                          = (known after apply)

      + advanced_security_options (known after apply)

      + auto_tune_options (known after apply)

      + cluster_config (known after apply)

      + domain_endpoint_options (known after apply)

      + ebs_options (known after apply)

      + encrypt_at_rest (known after apply)

      + node_to_node_encryption (known after apply)

      + off_peak_window_options (known after apply)

      + software_update_options (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_opensearch_domain.nautilus_es: Creating...
aws_opensearch_domain.nautilus_es: Still creating... [10s elapsed]
...
# One eternity later
...
aws_opensearch_domain.nautilus_es: Creation complete after 10m0s [id=arn:aws:es:us-east-1:000000000000:domain/nautilus-es]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

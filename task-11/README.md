# Task 11: Create Alarm Using Terraform

## Objective

The Nautilus DevOps team is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm to monitor their infrastructure and ensure optimal performance and availability

1. Create a CloudWatch alarm named `devops-alarm`
2. The alarm should be monitor `CPU Utilization` of an EC2 instance
3. Trigger the alarm when `CPU Utilization` exceeds `80%`
4. Set the evaluation period to `5 minutes`
5. Use a `single evaluation period`

## Solution

to set/create an alamr, let's write the following `main.tf`

```hcl
resource "aws_cloudwatch_metric_alarm" "devops_alarm" {
    alarm_name          = "devops-alarm"
    comparison_operator = "GreaterThanThreshold"
    evaluation_periods  = 1
    metric_name         = "CPUUtilization"
    namespace           = "AWS/EC2"
    period              = 300
    statistic           = "Average"
    threshold           = 80
}
```
Now let's create the alarm by using `init`, `plan` and `apply`
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

  # aws_cloudwatch_metric_alarm.devops_alarm will be created
  + resource "aws_cloudwatch_metric_alarm" "devops_alarm" {
      + actions_enabled                       = true
      + alarm_name                            = "devops-alarm"
      + arn                                   = (known after apply)
      + comparison_operator                   = "GreaterThanThreshold"
      + evaluate_low_sample_count_percentiles = (known after apply)
      + evaluation_periods                    = 1
      + id                                    = (known after apply)
      + metric_name                           = "CPUUtilization"
      + namespace                             = "AWS/EC2"
      + period                                = 300
      + statistic                             = "Average"
      + tags_all                              = (known after apply)
      + threshold                             = 80
      + treat_missing_data                    = "missing"
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply
# Output
Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_cloudwatch_metric_alarm.devops_alarm will be created
  + resource "aws_cloudwatch_metric_alarm" "devops_alarm" {
      + actions_enabled                       = true
      + alarm_name                            = "devops-alarm"
      + arn                                   = (known after apply)
      + comparison_operator                   = "GreaterThanThreshold"
      + evaluate_low_sample_count_percentiles = (known after apply)
      + evaluation_periods                    = 1
      + id                                    = (known after apply)
      + metric_name                           = "CPUUtilization"
      + namespace                             = "AWS/EC2"
      + period                                = 300
      + statistic                             = "Average"
      + tags_all                              = (known after apply)
      + threshold                             = 80
      + treat_missing_data                    = "missing"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_cloudwatch_metric_alarm.devops_alarm: Creating...
aws_cloudwatch_metric_alarm.devops_alarm: Creation complete after 1s [id=devops-alarm]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

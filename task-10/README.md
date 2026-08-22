# Task 10: Create Snapshot Using Terraform

## Objective

The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.

Create a snapshot of an existing volume named `xfusion-vol` in `us-east-1` region using terraform.

1) The name of the snapshot must be `xfusion-vol-ss`.

2) The description must be `Xfusion Snapshot`.

3) Make sure the snapshot status is `completed` before submitting the task.

## Solution

or this task, we need tu update `main.tf` by adding the `aws_ebs_snapshot` resource and referencing the instance using `k8s_volume.id`

```hcl
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name        = "xfusion-vol"
  }
}

resource "aws_ebs_snapshot" "volume_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Xfusion Snapshot"

  tags = {
    Name = "xfusion-vol-ss"
  }
}
```
Now let's create our Snapchot by using `init`, `plan` and `apply`
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
aws_ebs_volume.k8s_volume: Refreshing state... [id=vol-becc773a624d9d04a]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ebs_snapshot.volume_snapshot will be created
  + resource "aws_ebs_snapshot" "volume_snapshot" {
      ...
      + description            = "Xfusion Snapshot"
      ...
      + tags                   = {
          + "Name" = "xfusion-vol-ss"
        }
      + tags_all               = {
          + "Name" = "xfusion-vol-ss"
        }
      + volume_id              = "vol-becc773a624d9d04a"
      + volume_size            = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```
And finally `apply`
```bash
terraform apply
# Output
aws_ebs_volume.k8s_volume: Refreshing state... [id=vol-becc773a624d9d04a]

Terraform used the selected providers to generate the following execution plan. Resource
actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_ebs_snapshot.volume_snapshot will be created
  + resource "aws_ebs_snapshot" "volume_snapshot" {
      ...
      + description            = "Xfusion Snapshot"
      ...
      + tags                   = {
          + "Name" = "xfusion-vol-ss"
        }
      + tags_all               = {
          + "Name" = "xfusion-vol-ss"
        }
      + volume_id              = "vol-becc773a624d9d04a"
      + volume_size            = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_ebs_snapshot.volume_snapshot: Creating...
aws_ebs_snapshot.volume_snapshot: Creation complete after 0s [id=snap-6a5377e06ce1a4293]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

# TERRAFORM-AWS-AUTO-EBS-SNAPSHOT

Creates a lambda and cloudwatch event to look for any instances that have the specfied tag and then takes a snapshot of that volume.
This is a heavily modified version of this module: <https://github.com/kgorskowski/ebs_bckup>

## Usage

To implement add the following to your code:

``` HCL
module "ebsnapshot" {
  source          = "../../"
  common_tags     = "${var.common_tags}"
  snapshot_name   = "${local.snapshot_name}"
  cron_expression = "${var.cron_expression}"
  regions         = "${var.regions}"
}
```

and set up your Terraform tf vars file to look something like:

``` HCL
common_tags = {
    application = "terraform"
    module      = "auto-ebs-snapshot"
    environment = "develop"
}

EC2_INSTANCE_TAG = "Backup"
RETENTION_DAYS   = 10
cron_expression  = "45 1 * * ? *"
regions          = ["eu-west-1","eu-west-2","eu-west-3"]
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| EC2_INSTANCE_TAG | Tag to identify the EC2 target instances of the Lambda Function | string | `Backup` | no |
| RETENTION_DAYS | Numbers of Days that the EBS Snapshots will be stored (INT) | string | `5` | no |
| common_tags | Implements the common tags scheme | map | - | yes |
| cron_expression | Cron expression for firing up the Lambda Function | string | - | yes |
| regions | Which Regions to apply the snapshots too | list | - | yes |
| snapshot_name | The name of the snapshot | string | - | yes |

## Outputs

| Name | Description |
|------|-------------|
| aws_iam_role_arn |  |
| lambda_function_name |  |

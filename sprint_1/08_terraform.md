# Terraform

### Provider

Start by setting up your provider to define which cloud plugins our project will require to deploy the resources.

Create a new file called `provider.tf`

```javascript
provider "aws" {
  region = "eu-west-1"
}
```

### Main

Create a `main.tf` file define all the resources we are about to create, which is an S3 bucket named `talent-academy-account_id-tfstates-pascal`.

Fine more about the resource `aws_s3_bucket` from the [terraform documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)

```javascript
resource "aws_s3_bucket" "my_state_bucket" {
  bucket = "talent-academy-account_id-tfstates-pascal"

  versioning {
    enabled = true
  }

  tags = {
    Name        = "talent-academy-tfstates"
    Environment = "Test"
  }
}
```

### Initialize to get ready for a Plan

That should be enough to get started and verify that everything is setup properly.

```sh
# Start initialization
terraform init

# Run a plan to check your code
terraform plan
```

### Apply

When you are satisfied with the `plan`, you can deploy your changes with the `apply` command, and type in `yes` to execute:

```sh
terraform apply
```

## Backend

The creation of the S3 bucket allows us to use it as our backend to store our `terraform.tfsates`.

Crete a new file named `backend.tf` to configure the s3 bucket for the backend.

```javascript
terraform {
  backend "s3" {
    bucket = "talent-academy-account_id-tfstates-pascal"
    key    = "sprint1/week2/training-terraform/terraform.tfstates"
  }
}
```

Make sure to use the same `bucket name` as the one you have deployed before. The `key` is the location of the file where the `terraform.tfstates` needs to be stored inside the bucket.

Relaunch the initialization to allow terraform to apply the changes of the backend.

```sh
terraform init -reconfigure
```

## DynamoDB Lock

To avoid concurrent `apply` against the same infrastructure, it's best practice to use a `dynamodb table` to manage a lock file that will prevent multiple parallel deployment.

In the `main.tf` file, create a new resource for the `aws_dymanodb_table`.

```javascript
resource "aws_dynamodb_table" "terraform_lock_tbl" {
  name           = "terraform-lock"
  read_capacity  = 1
  write_capacity = 1
  hash_key       = "LockID"

  attribute {
    name = "LockID"
    type = "S"
  }

  tags           = {
    Name = "terraform-lock"
  }
}
```

Again, run another `terraform plan and apply`.

### Add lock to backend

Modify the `backend.tf` file again to add the new dynamo db table lock

```javascript
terraform {
  backend "s3" {
    bucket = "talent-academy-account_id-tfstates-pascal"
    key    = "sprint1/week2/training-terraform/terraform.tfstates"
    dynamodb_table = "terraform-lock"
  }
}
```
# Lambda

## Exercise 1 - Deploy a lambda function with Terraform

**Step 1**

- Create Role for lambda function with an assume Role
- Create a new policy to allow permission on cloudwatch logs:
```json
"logs:CreateLogGroup",
"logs:CreateLogStream",
"logs:PutLogEvents"
```
- Add permission to read and write to S3. You might need to create a new bucket first or use an existing.

**Step 2**

- Download the [sample json file](./sample_data.json) and upload it (using any method) into your S3 Bucket.

**Step 3**

- Create python Script
- Write small boto3 script to:
    - List all existing s3 buckets
    - read data from S3 based on the source event and print the output.
    - You can use the [AWS Sample python script](https://github.com/aws-samples/aws-python-sample/blob/master/s3_sample.py) as an example
    - Another example from here: [S3 Boto example](https://github.com/boto/boto3/blob/develop/boto3/examples/s3.rst)
- Using the Event input parameter below, find out and print the selected Pet favourite food:
```json
    {
        "S3Bucket" : <your bucket name>,
        "S3Prefix" : <Path to json file>,
        "PetName"  : <pet name as per json(choose any)>
    }
```

Examples:
```json
    {
        "S3Bucket" : "talent-academy-example-storage",
        "S3Prefix" : "sample_data.json",
        "PetName"  : "Meowsalot"
    }
```

**Step 4**

- Using terraform data source create a `.zip` file of the python script
- Create Lambda resource in terraform
    - Run time environment - Python 3.8

**Testing**

- Check if the above output you have printed appears in cloudwatch logs, if not assign IAM role correct permisions and execute the same set of instructions.
- Explore the attribute available to context for this lambda function and discuss possible cases when context attributes can be used.

## Exercise 2 - Using EventBrige to collect image metadata

**Step 1**

- Repeat same steps as from the previous exercise but the lambda function objective is different

**Step 2**

- Create a notification for S3 to trigger the lambda function everytime a new object is uploaded
    - Use [EventBridge rule -- formally known as CloudWatch rule](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule)
- Create a dynamoDB table in terraform
- You should be able to use the same role that was created in the previous exo. For this, you will need the [Data Source for iam role](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_role).
- Create a new policy to allow read and write access to the new DynamoDB.

**Step 3**

- In the python lambda function, collect the meta data of the image.
- Use the dynamoDB table to store the meta data for each image

**Bonus Stage**

- AWS has a Machine Learning service called [AWS Rekognition](https://aws.amazon.com/rekognition/) that allows you to detect objects pr people etc. Using the [sample codes](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-python-example_code-rekognition.html) try to update your python code to look for more information about the content of the image and store it in the dynamoDB as well.
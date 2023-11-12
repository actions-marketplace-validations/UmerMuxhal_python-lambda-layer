# Publish or Update AWS Lambda Layer for python

- [Objective](#objective)
- [Usage](#usage)
    - [Inputs](#inputs)
    - [Examples](#examples)
- [Published Layer Name](#published-layer-name)

## Objective

A Lambda layer is a .zip file archive that contains supplementary code or data. Layers usually contain library
dependencies, a custom runtime, or configuration files. Layers promote code sharing and separation of responsibilities
so that you can iterate faster on writing business logic.

Reasons to use layers are listed below:

- To reduce the size of your deployment packages.
- To separate core function logic from dependencies.
- To share dependencies across multiple functions.
- To use the Lambda console code editor.

Objective of this action is to streamline the process of publishing an AWS Lambda Layer in GitHub workflows, eliminating
the need to manually write code.

## Usage

Usage of this action requires following parameters.

### Inputs

| Name              | Type   | Required | Description                                                                                            |
|-------------------|--------|----------|--------------------------------------------------------------------------------------------------------|
| `python_version`  | string | No       | Python Version, default value is `3.7`                                                                 |
| `layer_name`      | string | Yes      | Name of AWS Lambda Layer                                                                               |
| `layer_directory` | string | Yes      | Working directory in repository where `requirements.txt` file exists                                   |
| `bucket_name`     | string | Yes      | S3 bucket name where lambda layer will be uploaded                                                     |
| `bucket_path`     | string | No       | Folder path inside the S3 bucket (Optional)                                                            |
| `aws_account_id`  | string | No       | An AWS Account ID to grant layer usage permissions to, default value is `*` to share with all acoounts |

The `python_version` only accepts versions of python mentioned in AWS Lambda
docs; [Lambda runtimes](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html).

The `layer-name` can be anything you prefer. This is restricted to letters, numbers, hyphens, and underscores.

The `layer-directory` is the folder path in git repository where `requirements.txt` file exists.

The `bucket_name` parameter is the name of S3 bucket in which you want to upload the lambda layer.

The `bucket_path` parameter is the folder path for lambda layer in S3 bucket.

The `aws_account_id` parameter is an AWS account number with which you want to share the lambda layer. To share it with all accounts, use "*".

## Published Layer Name

The ARN of published lambda layer will look like the following.

arn:aws:lambda:`<AWS_REGION>`:`<AWS_ACCOUNT_ID>`:layer:`<layer_name>`-`<python_version>`:`<layer_version>`


# Publish or Update AWS Lambda Layer for python

![Static Badge](https://img.shields.io/badge/v1-brightgreen?style=flat-square&logo=python&logoColor=yellow&label=lambda-layer&link=https%3A%2F%2Fgithub.com%2FUmerMuxhal%2Fpython-lambda-layer%2Ftree%2Fv1)
[![GitHubActions](https://img.shields.io/badge/listed%20on-GitHubActions-blue.svg)](https://github.com/marketplace/actions/python-lambda-layer)

- [Objective](#objective)
- [Usage](#usage)
    - [Inputs](#inputs)
    - [Example](#example)
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
| `layer_directory` | string | No       | Path of `requirements.txt` file in repository.                                                         |
| `bucket_name`     | string | Yes      | S3 bucket name where lambda layer will be uploaded                                                     |
| `bucket_path`     | string | No       | Folder path inside the S3 bucket (Optional)                                                            |
| `aws_account_id`  | string | No       | An AWS Account ID to grant layer usage permissions to, default value is `*` to share with all acoounts |

The `python_version` only accepts versions of python mentioned in AWS Lambda
docs; [Lambda runtimes](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html).

The `layer_name` can be anything you prefer. This is restricted to letters, numbers, hyphens, and underscores.

The `layer_directory` is the folder path in git repository where `requirements.txt` file exists. It is optional in
case `requirements.txt` file is in root of repository.

The `bucket_name` parameter is the name of S3 bucket in which you want to upload the lambda layer.

The `bucket_path` parameter is the folder path for lambda layer in S3 bucket. Use the format `lambda-layers/` to add S3
path.

The `aws_account_id` parameter is an AWS account number with which you want to share the lambda layer. To share it with
all accounts, use "*".

### Example

```yaml
name: publish-python-lambda-layer
on:
  push:
    branches:
      - dev
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Publish Lambda layer
        uses: UmerMuxhal/python-lambda-layer@v1
        with:
          python_version: 3.8
          layer_name: "my-lambda-layer"
          layer_directory: "requirements_folder"
          bucket_name: "my-s3-bucket"
          bucket_path: "layers/"
          aws_account_id: ${{ secrets.AWS_ACCOUNT_ID }}
```

## Published Layer Name

The ARN of published lambda layer will look like the following.

arn:aws:lambda:`<AWS_REGION>`:`<AWS_ACCOUNT_ID>`:layer:`<layer_name>`-`<python_version>`:`<layer_version>`


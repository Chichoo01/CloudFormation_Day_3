# Working with AWS CloudFormation StackSets

- AWS CloudFormation StackSets extends the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation.
- Using an administrator account, you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts across specified regions.



## StackSets Concepts 

- Administrator and target accounts
- Stack sets
- Stack instances
- Stack set operations
- Stack set operation options
- Tags
- Stack set and stack instance status codes

## Prerequisites for Stack Set Operations

Because stack sets perform stack operations across multiple accounts, before you can create your first stack set you need the necessary permissions defined in your AWS accounts.

- Grant Self-Managed Permissions (Self Managed)
- Enable Trusted Access with AWS Organizations (Service Managed)

## Grant Self-Managed Permissions

- Before you create a stack set with self-managed permissions, you need to establish a trust relationship between the administrator and target accounts by creating IAM roles in each account.
- In the administrator account, create an IAM role named **AWSCloudFormationStackSetAdministrationRole**. 

The role must have this exact name. You can do this by creating a stack from the following AWS CloudFormation template, available online at https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetAdministrationRole.yml.

The role created by this template enables the following policy on your administrator account.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/AWSCloudFormationStackSetExecutionRole"
            ],
            "Effect": "Allow"
        }
    ]
}
```

The following trust relationship is created by the preceding template.
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudformation.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

- In each target account, create a service role named **AWSCloudFormationStackSetExecutionRole** that trusts the administrator account.

The role must have this exact name. You can do this by creating a stack from the following AWS CloudFormation template, available online at https://s3.amazonaws.com/cloudformation-stackset-sample-templates-us-east-1/AWSCloudFormationStackSetExecutionRole.yml. When you use this template, you are prompted to provide the name of the administrator account with which your target account must have a trust relationship.

The role created by this template enables the following policy on a target account.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```
The following trust relationship is created by the template. The administrator account's ID is shown as admin_account_id.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::admin_account_id:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
## Getting Started with AWS CloudFormation StackSets

- Create a Stack Set
- Update Your Stack Set
- Add Stacks to a Stack Set
- Override Parameters on Stack Instances
- Delete Stack Instances From Your Stack Set
- Delete a Stack Set


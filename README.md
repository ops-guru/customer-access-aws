# Repository Overview: AWS Access Templates

This repository contains example CloudFormation templates designed to grant access to OpsGuru employees in your AWS account. Each template offers varying levels of access, allowing you to tailor permissions to your specific needs.

The `CrossAccountRoleWithManagedPolicy.yaml` template leverages a specified AWS managed policy to provide permissions in your AWS account. You may chose from one of the following AWS managed policies:

- `AdministratorAccess`: Provides comprehensive administrative privileges across all resources in your AWS account
- `AmazonEC2ReadOnlyAccess`: Provides read only access to Amazon EC2
- `AWSBillingReadOnlyAccess`: Provides read-only access exclusively to billing information within your AWS account
- `ReadOnlyAccess`: Provides read-only accesss to all resources in your AWS account

For more granular control over resources and permissions, explore the example files within the `inline-policy-examples` folder. These templates demonstrate the use of inline policies, allowing you to finely tune access according to your requirements.

Feel free to review and choose the template that best aligns with your security and access management strategy. If additional customization is needed, refer to the inline policy examples for inspiration on crafting policies tailored to your specific use case.

If you're unsure about which template precisely aligns with your project requirements, we recommend using the `ReadOnlyAccess.json` template. This template grants read-only access to all your services and resources, catering to the needs of OpsGuru team members.

## Required Information for CloudFormation Template Customization

To effectively use the provided CloudFormation templates, specify the following parameters with accurate information for seamless integration with your AWS account:

Parameter  | Description
---------  | -----------
RoleName   | Name of the IAM role to create in your AWS account
Principal  | Amazon Resource Name (ARN) of the principal that can assume the IAM role
ManagedPolicyArn | AWS managed policy to attach to the IAM role


Kindly request the OpsGuru team to furnish you with the specific values for the parameters mentioned above.

## Configuring Multiple Access Levels 

If you require diverse access levels for various groups within the OpsGuru team in your AWS account, utilize distinct templates to assign permissions to different roles. Collaborate with the OpsGuru team to establish multiple roles as needed.

## How to Utilize Templates for Access Provisioning

To employ the provided templates, follow these steps:

1. Navigate to the CloudFormation page in your AWS Managemenet Console.
2. Click on the `Create stack` > `With new resources (standard)` button.
3. In the subsequent steps, under `Specify template`, opt for the `Upload a template file` radio button, click `Choose file` and select the corresponding template file from this repository. The template will be automatically uploaded to an AWS-managed S3 bucket in your account for CloudFormation templates.
4. Click `Next` and provide a name for your stack (e.g., `OpsGuru-readonly-billing`).
5. Specify the `ManagedPolicyArn`, `Principal` and `RoleName` parameters as provided by OpsGuru.
6. Click `Next`, optionally add tags to the stack, and click `Next` again.
7. In the last step, acknowledge that `AWS CloudFormation might create IAM resources with custom names` by checking the corresponding box.
8. Click `Submit`. The stack will be generated and commence execution. Upon successful completion, you should see the status `CREATE_COMPLETE` for this stack.


## Access Revocation

To revoke OpsGuru team access to your AWS account, simply delete the CloudFormation stack created in the previous step. By removing this stack, all associated resources (i.e. IAM Role) providing access to your AWS account will be seamlessly removed.
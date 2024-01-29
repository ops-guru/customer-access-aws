# Repository Overview: AWS Access Templates

This repository contains JSON files with example CloudFormation templates designed to grant access to Opsguru employees in your AWS account. Each template offers varying levels of access, allowing you to tailor permissions to your specific needs.

As an illustration, consider the `aws-AdministratorAccess.json` file. This template leverages the `AdministratorAccess` AWS Managed Policy, providing comprehensive administrative privileges across all resources in your AWS account. In contrast, the `aws-BillingReadOnlyAccess.json` file is designed to provide read-only access exclusively to billing information within your AWS account. Moreover, the `aws-ReadOnlyAccess.json` file provides the read-only accesss to all resources in your AWS account. 

For more granular control over resources and permissions, explore the example files within the `inline-policy-examples` folder. These templates demonstrate the use of inline policies, allowing you to finely tune access according to your requirements.

Feel free to review and choose the template that best aligns with your security and access management strategy. If additional customization is needed, refer to the inline policy examples for inspiration on crafting policies tailored to your specific use case.

## Required Information for JSON File Customization

To effectively use the provided JSON files, ensure to replace the following parameters with accurate information for seamless integration with your AWS account:

Parameter  | Description
---------  | -----------
RoleName   | Name of the IAM Role to be created in your AWS account
Principal  | ARN (Amazon Resource Name) that can assume the created Role in your account


Kindly request the Opsguru team to furnish you with the specific values for the parameters mentioned above.

## Configuring Multiple Access Levels 

If you require diverse access levels for various groups within the Opsguru team in your AWS account, utilize distinct templates to assign permissions to different roles. Collaborate with the Opsguru team to establish multiple roles as needed.

## How to Utilize Templates for Access Provisioning

To employ the provided templates, follow these steps after updating the `RoleName` and `Principal` parameters:

1. Navigate to the CloudFormation page in your AWS Console.
2. Click on the `Create stack` button.
3. In the subsequent steps, under `Specify template`, opt for the `Upload a template file` radio button, and select the corresponding template file from this repository. The template will be automatically uploaded to a newly created or existing S3 bucket for CloudFormation templates.
4. Click `Next` and provide a name for your stack (e.g., opsguru-readonly-billing). Continue to click `Next` until you reach the final step.
5. In the last step, acknowledge that `AWS CloudFormation might create IAM resources with custom names` by checking the corresponding box.
6. Click `Submit`. The stack will be generated and commence execution. Upon successful completion, you should see the status `CREATE_COMPLETE` for this stack.


## Access Revocation

To revoke Opsguru team access to your AWS account, simply delete the CloudFormation stack created in the previous step. By removing this stack, all associated resources (i.e. IAM Role) providing access to your AWS account will be seamlessly removed.
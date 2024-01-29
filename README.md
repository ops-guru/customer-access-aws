# Repository Overview: AWS Access Templates

This repository contains JSON files with example CloudFormation templates designed to grant access to Opsguru employees in your AWS account. Each template offers varying levels of access, allowing you to tailor permissions to your specific needs.

As an illustration, consider the aws-AdministratorAccess.json file. This template leverages the AdministratorAccess AWS Managed Policy, providing comprehensive administrative privileges across all resources in your AWS account.

For more granular control over resources and permissions, explore the example files within the inline-policy-examples folder. These templates demonstrate the use of inline policies, allowing you to finely tune access according to your requirements.

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
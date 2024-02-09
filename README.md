# Repository Overview: AWS Access Templates

This repository contains example CloudFormation templates designed to grant access to OpsGuru employees in your AWS account. Each template offers varying levels of access, allowing you to tailor permissions to your specific needs.

The `CrossAccountRoleWithManagedPolicy.yaml` template leverages a specified AWS managed policy to provide permissions in your AWS account. You may chose from one of the following AWS managed policies:

- `AdministratorAccess`: Provides comprehensive administrative privileges across all resources in your AWS account
- `AmazonEC2ReadOnlyAccess`: Provides read only access to Amazon EC2
- `AWSBillingReadOnlyAccess`: Provides read-only access exclusively to billing information within your AWS account
- `ReadOnlyAccess`: Provides read-only accesss to all resources in your AWS account

For more granular control over resources and permissions, explore the example files within the `inline-policy-examples` folder. These templates demonstrate the use of inline policies, allowing you to finely tune access according to your requirements.

Feel free to review and choose the template that best aligns with your security and access management strategy. If additional customization is needed, refer to the inline policy examples for inspiration on crafting policies tailored to your specific use case.

If you're unsure about which template precisely aligns with your project requirements, we recommend using the `CrossAccountRoleWithManagedPolicy.yaml` template with the `ReadOnlyAccess` managed policy. This template grants read-only access to all your services and resources, catering to the needs of OpsGuru team members.

## Required Information for CloudFormation Template Customization

To effectively use the provided `CrossAccountRoleWithManagedPolicy.yaml` CloudFormation template, specify the following parameters with accurate information for seamless integration with your AWS account:

Parameter  | Description
---------  | -----------
RoleName   | Name of the IAM role to create in your AWS account
Principal  | Amazon Resource Name (ARN) of the principal that can assume the IAM role
ManagedPolicyArn | AWS managed policy to attach to the IAM role

Kindly request the OpsGuru team to furnish you with the specific values for the parameters mentioned above.

## Configuring Multiple Access Levels 

If you require diverse access levels for various groups within the OpsGuru team in your AWS account, create distinct CloudFormation stacks to assign permissions to different roles. Collaborate with the OpsGuru team to establish multiple roles as needed.

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

To revoke OpsGuru team access to your AWS account, simply delete the CloudFormation stack created in the previous step. By removing this stack, all associated resources (i.e., IAM role) providing access to your AWS account will be seamlessly removed.


## CloudTrail - Logging of AWS users activity

1. Go to the CloudTrail console.
- Click on "Trails" in the sidebar.
- Click on "Create trail".
- Enter a name for your trail.
- Choose "Apply trail to all regions" if you want to log activities across all regions.
- Choose "Log all S3 data events" if you want to log S3 data events.
- Choose "Create a new S3 bucket" or select an existing one to store your logs.
- Under "Management events", select "Read/Write events" to log management events like IAM changes.
- Enable CloudTrail Insights (Optional):

CloudTrail Insights can help you identify unusual activity in your AWS account by analyzing CloudTrail logs.
You can enable it from the CloudTrail console by selecting the trail and then clicking on "Insights" in the navigation pane.

### Filter CloudTrail events by IAM Role

Once your trail is created, you can set up event selectors to filter the events you want to log.
- Click on your trail in the CloudTrail console.
- Under "Data events", click "Add data event".
- Choose the services and operations you want to log (e.g., S3 operations).
- Under "Specific resource ARN", enter the ARN of the IAM Role you want to filter by.
- Click "Save".
- Review and Analyze Logs:

CloudTrail logs can be accessed via the S3 bucket you specified.
You can also use CloudWatch Logs to monitor and set up alarms for specific events.
Use CloudTrail Insights to analyze logs for unusual activity and potential security threats.

By following these steps, you can create and use AWS CloudTrail to log all activities performed by arbitrary users and filter them by a specific IAM Role. This helps you maintain visibility and control over actions taken in your AWS account.


#### Use Case: Finding EC2 Service Changes

**CloudTrail Query:**
```sql
SELECT *
FROM cloudtrail_logs
WHERE eventSource = 'ec2.amazonaws.com'
```

**Explanation**: This query searches through CloudTrail logs to find all events related to the EC2 service. The eventSource field in CloudTrail logs indicates the AWS service that generated the event. By filtering events where eventSource is equal to 'ec2.amazonaws.com', you can identify all activities related to EC2 service changes.

#### Use Case: Finding Read/Write Events of a Specific IAM Role

**CloudTrail Query:**


```sql
SELECT *
FROM cloudtrail_logs
WHERE eventSource = 's3.amazonaws.com'
AND eventName LIKE 'GetObject%'
AND userIdentity.arn = 'arn:aws:iam::<edit-account-id>:role/your-iam-role'
```
**Explanation**: This query looks for all CloudTrail events related to Read (GetObject) and Write (PutObject, DeleteObject, etc.) events on an S3 bucket by a specific IAM Role. It filters events where eventSource is 's3.amazonaws.com' and eventName starts with 'GetObject' (for Read events). Additionally, it specifies the IAM Role's ARN in the userIdentity.arn field to identify events performed by that IAM Role.

These examples demonstrate how you can use CloudTrail queries to search for specific types of events or activities within your AWS environment, providing insights into changes and actions performed by users or services.

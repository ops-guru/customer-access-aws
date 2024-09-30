# OpsGuru Customer Access: AWS

This repository contains a CloudFormation template designed to grant access to OpsGuru employees in your AWS account. The template offers varying levels of access, allowing you to tailor permissions to your specific needs via the parameter values you provide.

The `CrossAccountRoleWithManagedPolicy.yaml` template leverages a specified [AWS managed policy for job function](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) to provide permissions in your AWS account. You may chose from one of the following AWS managed policies:

- `AdministratorAccess`: Provides comprehensive administrative privileges across all resources in your AWS account
- `AmazonEC2ReadOnlyAccess`: Provides read only access to Amazon EC2
- `AWSBillingReadOnlyAccess`: Provides read-only access exclusively to billing information within your AWS account
- `ReadOnlyAccess`: Provides read-only access to all resources in your AWS account
- `Security-Audit-with-Prowler`: Provides two AWS managed policies, `SecurityAudit` and `job-function/ViewOnlyAccess`, to enable Prowler to run security audits on your AWS account.

For more granular control over resources and permissions, explore the example files within the `inline-policy-examples` folder. These templates demonstrate the use of inline policies, allowing you to finely tune access according to your requirements.

Feel free to review and choose the AWS managed policy that best aligns with your security and access management strategy. If additional customization is needed, refer to the inline policy examples for inspiration on crafting policies tailored to your specific use case.

If you're unsure about which policy aligns with your project requirements, we recommend using the `CrossAccountRoleWithManagedPolicy.yaml` template with the `ReadOnlyAccess` managed policy. This template grants read-only access to all your services and resources, catering to the needs of OpsGuru team members.

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

1. Navigate to the CloudFormation page in your AWS Management Console.
2. Click on the `Create stack` > `With new resources (standard)` button.
3. In the subsequent steps, under `Specify template`, opt for the `Upload a template file` radio button, click `Choose file` and select the corresponding template file from this repository. The template will be automatically uploaded to an AWS-managed S3 bucket in your account for CloudFormation templates.
4. Click `Next` and provide a name for your stack (e.g., `OpsGuru-readonly-billing`).
5. Specify the `ManagedPolicyArn`, `Principal` and `RoleName` parameters as provided by OpsGuru.
6. Click `Next`, optionally add tags to the stack, and click `Next` again.
7. In the last step, acknowledge that `AWS CloudFormation might create IAM resources with custom names` by checking the corresponding box.
8. Click `Submit`. The stack will be generated and commence execution. Upon successful completion, you should see the status `CREATE_COMPLETE` for this stack.

## Access Revocation

To revoke OpsGuru team access to your AWS account, simply delete the CloudFormation stack created in the previous step. By removing this stack, all associated resources (i.e., IAM role) providing access to your AWS account will be seamlessly removed.

## CloudTrail: Logging of AWS Activity

Actions performed by the provisioned IAM roles are logged by AWS CloudTrail. CloudTrail Event history makes all management events available for 90 days by default. If you want to store more than 90 days of history, or also record data or Insights event types, create a CloudTrail trail by following the directions in the [Creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html) section of the AWS CloudTrail User Guide.

### CloudTrail Data and Insight Events

CloudTrail trails records all read and write management events by default. If you also wish to log data or Insights event types, refer to the following sections of the AWS CloudTrail User Guide:

1. [Logging data events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)
2. [Logging Insights events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-insights-events-with-cloudtrail.html)

### Review and Analyze Logs

There are multiple ways to analyze the log data made available through CloudTrail. The simplest, and available by default, is to search for events through the CloudTrail Event history. See [Working with CloudTrail Event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) for details on this method.

Once you've created a trail that saves to an S3 bucket, you can create a table in Amazon Athena and use robust SQL queries against the events saved to the trail. See [Querying AWS CloudTrail logs](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html) for more details.

If you also configured a trail to save to CloudWatch Logs, you can view, filter, create metrics and alert on CloudTrail events using CloudWatch. See [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/monitor-cloudtrail-log-files-with-cloudwatch-logs.html) for details.

Finally, the most advanced option for querying CloudTrail log data is using SQL queries against a CloudTrail Lake. See [Working with AWS CloudTrail Lake](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-lake.html) for more details.

By using one of the methods above, you can search CloudTrail logs for activities performed against specific services or by a specific IAM role. This helps you maintain visibility and control over actions taken in your AWS account.

#### SQL Query Examples

These examples demonstrate how you use SQL queries via Athena or CloudTrail Lake to search for specific types of CloudTrail events or activities within your AWS environment, providing insights into changes and actions performed by users or services.

_Note: The following queries assume your Athena table or CloudTrail Lake event data store ID is `cloudtrail_logs`._

**Use Case: Finding EC2 service actions:**

```sql
SELECT *
FROM cloudtrail_logs
WHERE eventSource = 'ec2.amazonaws.com'
```

This query searches through CloudTrail logs to find all events related to the EC2 service. The `eventSource` field in CloudTrail logs indicates the AWS service that generated the event. By filtering events where `eventSource` is equal to `ec2.amazonaws.com`, you can identify all activities related to the EC2 service.

**Use Case: Finding S3 object read events performed by a specific IAM role:**

```sql
SELECT *
FROM cloudtrail_logs
WHERE eventSource = 's3.amazonaws.com'
AND eventName LIKE 'GetObject%'
AND userIdentity.arn = 'arn:aws:iam::<AWS account ID>:role/<IAM role name>'
```

This query looks for all CloudTrail S3 object read events (`GetObject*`) performed by a specific IAM role. It filters events where `eventSource` is `s3.amazonaws.com` and `eventName` starts with `GetObject`. Additionally, it specifies the IAM role's ARN in the `userIdentity.arn` field to identify events performed by that IAM role.
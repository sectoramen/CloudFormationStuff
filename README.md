# My CloudFormation Stacks

Automation makes life easier. Enjoy my cloudformation stacks!

## ProwlerEC2toS3withSSMandSQS-3Modes

A CloudFormation stack to run prowler on an EC2 instance. Depending on the CloudFormation parameters used, prowler will run as follows:

- **if AWSOrganizations** is set to **true** then the AWSAccounts Parameter will be ignored. This parameter takes over the **AWSAccounts** parameter. In this case you must run this CloudFormation stack on the AWS root account. Prowler will run against all accounts in the Organization
- **if: AWSOrganizations** is set to **false** and **AWSAccounts** contains one or more AWS accounts (separated by space), then Prowler will run against the listed accounts.
- **if AWSOrganizations** is set to **false** and **AWSAccounts** is **empty**, then Prowler will run on the same account you are deploying the stack on.

The stack creates the following Resources:

- **All the Networking components:** a VPC, a Public and Private Subnet, an Internet Gateway, a NAT Gateway, and the Routing Tables.
- **All the Security Stuff:** SG with no inbound and only TCP 443 outbound, IAM Policy and Role to Run Prowler locally with Assume Role and access the EC2 instance via SSM session manager.
- **S3 Bucket:** to store the prowler output and log. The bucket is private and it will trigger an Email SNS notification when the files are ready
- **EC2 Instance:** Patched and with all the requirements to run prowler. Prowler's output and log are sent to S3 and this instance will shut down once prowler completes.
- **SNS:** topic, subscription and policy for email notification. Check your inbox while the CloudFormation stack is creating resources and accept the topic subscription before prowler completes its execution to receive notification.

You can access the EC2 instance using SSM Session Manager if you want to troubleshoot or as needed. Download the files from the S3 bucket after receiving the email notification and then delete the files. Once the files are deleted from S3, you can safely delete the CloudFormation template, which will remove all resources.

**The next three CloudFormation stacks break down the one above. I started with the three below and then combined them to the one above while I was learning.**

### ProwlerEC2toS3withSSMandSQS

Same as above but only for the account in which the CLoudFormation stack is executed. You must have the right permissions and be able to assume role on these accounts.

**NOTE**: Do NOT add -R and -A as Prowler parameters.

### ProwlerMultiAcctEC2toS3withSSMandSQS

Same as above but for multiple accounts specified as CloudFormation parameters. You can specify a *space* separated list of accounts to scan. You must have the right permissions and be able to assume role on these accounts.

**NOTE**: Do NOT add -R and -A as Prowler parameters.

### ProwlerOrgEC2toS3withSSMandSQS

Same as above but for multiple accounts in a AWS Organizations. You must have the right permissions and be able to assume role on these accounts.

**NOTE**: This CloudFormation stack must be run on the root account. Do NOT add -R and -A as Prowler parameters.
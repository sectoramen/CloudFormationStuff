# My CloudFormation Stacks

Automation makes life easier. Enjoy my cloudformation stacks!

## ProwlerOnEC2

A cloudformation stack to run prowler on an EC2 instance. The stack creates the following Resources:
- **All the Networking components:** a VPC, a Public and Private Subnet, an Internet Gateway, a NAT Gateway, and the Routing Tables.
- **All the Secuyrity Stuff:** SG with no inbound and only TCP 443 outbound, IAM Policy and Role to Run Prowler locally with Assume Role and access the EC2 instance via SSM session manager.
- **S3 Bucket:** to store the prowler output and log. The bucket is private and it will trigger an Email SNS notification when the files are ready
- **EC2 Instance:** Patched and with all the requirements to run prowler. Prowler's output and log are sent to S3 and this instance will shut down once prowler completes.
- **SNS:** topic, subscription and policy for email notification. Check your inbox while the CloudFormation stack is creating resources and accept the topic subscription before prowler completes its execution to receive notification.

You can access the EC2 instace using SSM Session Manager if you want to troubleshoot or as needed. Download the files from the S3 bucket after receving the email notification and then delete the files. Once the files are deleted from S3, you can safely delete the CloudFormation template, which will remove all resources.

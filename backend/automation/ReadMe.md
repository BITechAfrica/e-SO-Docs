# HOW WE AUTOMATED THE BACKEND DEPLOYMENT

Technologies

Springboot 3.3.2

Github Actions - YAML

### STEP 1:Create an AWS IAM role for GitHub Actions (OIDC)

> Goal: let GitHub Actions deploy to your EB envs without long-lived AWS keys.

A) Add the GitHub OIDC identity provider (once per AWS account)
AWS Console → IAM → Identity providers → Add provider → OpenID Connect
Provider URL: https://token.actions.githubusercontent.com → Get thumbprint
Audience: sts.amazonaws.com → Add provider

<img width="648" height="617" alt="image" src="https://github.com/user-attachments/assets/bb424c8a-9b3b-4656-9b00-64b2d1e0a7a3" />
<img width="648" height="617" alt="image" src="https://github.com/user-attachments/assets/d7c802f2-dc3d-4239-ad0c-cc3be8feefc0" />

B) Create the deploy role (assumed by your workflows)
> IAM → Roles → Create role → Web identity
> Identity provider: token.actions.githubusercontent.com
> Audience: sts.amazonaws.com
<img width="701" height="705" alt="image" src="https://github.com/user-attachments/assets/827f6212-dd99-4907-9f72-2d5853ea60c3" />

#### Trust policy:
> paste this JSON, but replace ORG/REPO with your GitHub org/repo name. This limits role assumption to pushes on dev and test branches.

#### ✅ What does this policy do?
> It grants GitHub Actions workflows (from repositories in the BITechAfrica organization) permission to assume the IAM Role without storing long-term AWS credentials. Instead, it uses a short-lived token provided by GitHub’s OIDC provider.
---
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::557690591577:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": "repo:BITechAfrica/*"
                }
            }
        }
    ]
}
```
C) Attach least-privilege permissions

On the role page → Permissions tab → Add permissions → Create inline policy → JSON.

Paste this (replace <ACCOUNT_ID> if you know your EB bucket name; otherwise keep both lines):

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "ElasticBeanstalkCore",
			"Effect": "Allow",
			"Action": [
				"elasticbeanstalk:CreateApplicationVersion",
				"elasticbeanstalk:UpdateEnvironment",
				"elasticbeanstalk:Describe*",
				"elasticbeanstalk:List*"
			],
			"Resource": "*"
		},
		{
			"Sid": "S3AppVersionObjects",
			"Effect": "Allow",
			"Action": [
				"s3:PutObject",
				"s3:GetObject",
				"s3:DeleteObject",
				"s3:AbortMultipartUpload"
			],
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577/*"
		},
		{
			"Sid": "S3BucketOwnershipControls",
			"Effect": "Allow",
			"Action": [
				"s3:GetBucketOwnershipControls",
				"s3:PutBucketOwnershipControls"
			],
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577"
		},
		{
			"Sid": "S3ListBucket",
			"Effect": "Allow",
			"Action": "s3:ListBucket",
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577"
		},
		{
			"Sid": "S3CreateBucketOnce",
			"Effect": "Allow",
			"Action": "s3:CreateBucket",
			"Resource": "*"
		},
		{
			"Sid": "CloudFormationReadForEB",
			"Effect": "Allow",
			"Action": [
				"cloudformation:GetTemplate",
				"cloudformation:GetTemplateSummary",
				"cloudformation:DescribeStacks",
				"cloudformation:DescribeStackEvents",
				"cloudformation:DescribeStackResource",
				"cloudformation:DescribeStackResources",
				"cloudformation:ListStackResources"
			],
			"Resource": "*"
		},
		{
			"Sid": "AutoScalingReadForEB",
			"Effect": "Allow",
			"Action": [
				"autoscaling:DescribeAutoScalingGroups",
				"autoscaling:DescribeAutoScalingInstances",
				"autoscaling:DescribeScalingActivities"
			],
			"Resource": "*"
		},
		{
			"Sid": "S3AclReadForEB",
			"Effect": "Allow",
			"Action": [
				"s3:GetObjectAcl"
			],
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577/*"
		},
		{
			"Sid": "EBRestartAppServer",
			"Effect": "Allow",
			"Action": [
				"elasticbeanstalk:RestartAppServer"
			],
			"Resource": [
				"arn:aws:elasticbeanstalk:af-south-1:557690591577:environment/epmo/dev-epmo",
				"arn:aws:elasticbeanstalk:af-south-1:557690591577:environment/epmo/Epmo-env"
			]
		},
		{
			"Sid": "AutoScalingReadOnly",
			"Effect": "Allow",
			"Action": [
				"autoscaling:DescribeAutoScalingGroups",
				"autoscaling:DescribeAutoScalingInstances",
				"autoscaling:DescribeScalingActivities",
				"autoscaling:DescribeLaunchConfigurations",
				"autoscaling:DescribePolicies"
			],
			"Resource": "*"
		},
		{
			"Sid": "S3AclReadWriteForEB",
			"Effect": "Allow",
			"Action": [
				"s3:GetObjectAcl",
				"s3:PutObjectAcl"
			],
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577/*"
		},
		{
			"Sid": "S3BucketPolicyRead",
			"Effect": "Allow",
			"Action": [
				"s3:GetBucketPolicy"
			],
			"Resource": "arn:aws:s3:::elasticbeanstalk-af-south-1-557690591577"
		},
		{
			"Sid": "AutoScalingSuspendResumeForEB",
			"Effect": "Allow",
			"Action": [
				"autoscaling:SuspendProcesses",
				"autoscaling:ResumeProcesses"
			],
			"Resource": "arn:aws:autoscaling:af-south-1:557690591577:autoScalingGroup:*:autoScalingGroupName/awseb-e-jkur3dpkrc-stack-AWSEBAutoScalingGroup-FbRZ2dlHSi3t"
		}
	]
}
```

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
See screenshot examples below
<img width="648" height="617" alt="image" src="https://github.com/user-attachments/assets/bb424c8a-9b3b-4656-9b00-64b2d1e0a7a3" />
<img width="648" height="617" alt="image" src="https://github.com/user-attachments/assets/d7c802f2-dc3d-4239-ad0c-cc3be8feefc0" />

B) Create the deploy role (assumed by your workflows)
> IAM → Roles → Create role → Web identity
> Identity provider: token.actions.githubusercontent.com
> Audience: sts.amazonaws.com
<img width="701" height="705" alt="image" src="https://github.com/user-attachments/assets/827f6212-dd99-4907-9f72-2d5853ea60c3" />
> Trust policy: paste this JSON, but replace ORG/REPO with your GitHub org/repo name. This limits role assumption to pushes on dev and test branches.
> ✅ What does this policy do? It grants GitHub Actions workflows (from repositories in the BITechAfrica organization) permission to assume the IAM Role without storing long-term AWS credentials. Instead, it uses a short-lived token provided by GitHub’s OIDC provider.
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


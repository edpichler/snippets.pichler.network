---
title: "AWS"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# AWS

## AWS profiles

| Command                                |                                                                                |
|----------------------------------------| ------------------------------------------------------------------------------ |
| `aws configure list-profiles`          | List the local configured aws cli profiles.                                    | 
| `aws sso login --profile my-profile`   | To login in using a profile and the SSO (with IAM Identity Center credentials) |
| `aws sso logout`                       | To logout. |
| `aws s3 ls --profile=myProfile`        | List buckets using a profile | 
| `export AWS_DEFAULT_PROFILE=myProfile` | To set up the default aws profile. |

## AWS IAM
| Command                                                   |                                                                                |
|-----------------------------------------------------------| ------------------------------------------------------------------------------ |
| `aws iam list-roles`                                      | List all roles in the account. |
| `aws iam get-role --role-name my-role`                    | Get the details of a role. |
| `aws iam list-attached-role-policies --role-name my-role` | List all policies attached to a role. |

## AMS EKS
| Command                                                   |                                                                                                     |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `aws eks list-clusters`                                   | List all EKS clusters.                                                                              |
| `aws eks describe-cluster --name my-cluster`              | Get the details of a cluster.                                                                       |
| `aws eks update-kubeconfig --name my-cluster`             | Update the `kubeconfig` file to access the cluster. Do `kubectl get svc` to test your configuration |

## AWS Policies
| Command                                                                                         |                                   |
|-------------------------------------------------------------------------------------------------|-----------------------------------|
| `aws iam list-policies`                                                                         | List all policies in the account. |
| `aws iam get-policy --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy` | Get the details of a policy.      |
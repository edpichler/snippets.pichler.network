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

## AWS profiles

| Name                                      |                                                                                |
| ----------------------------------------- | ------------------------------------------------------------------------------ |
| `aws configure list-profiles`             | List the local configured aws cli profiles.                                    | 
| `aws sso login --profile my-profile`      | To login in using a profile and the SSO (with IAM Identity Center credentials) |
| `aws sso logout`                          | To logout. |
| `aws s3 ls --profile=myProfile`           | List buckets using a profile | 
| `export AWS_DEFAULT_PROFILE=myProfile`    | To set up the default aws profile.
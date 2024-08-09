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

## To list the local configured aws cli profiles

`aws configure list-profiles`

## To sign in through the AWS CLI with IAM Identity Center credentials

To login: `aws sso login --profile my-profile`
And `aws sso logout` to logout.


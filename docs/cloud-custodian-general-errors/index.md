---
title: "Cloud Custodian General Errors"
date: "2018-06-14"
categories: 
  - "devops-tools"
---

This blog is providing the solution of general errors which you might face while configuring cloud custodian

**Error 1**

c7n\_org:ERROR Exception running policy:invalid-ip-address-login-detected account:sandbox-02 region:us-east-1 error:An error occurred (InvalidParameterValueException) when calling the CreateFunction operation: The role defined for the function cannot be assumed by Lambda.

This error you can get when you have mentioned role at two places like in account.yaml file where all account related configuration is present and the policy file.  All roles should be configured in yaml file only not on the policy file

**Error 2**

assert role, "Lambda function role must be specified" AssertionError: Lambda function role must be specified

This error is because you don't have IAM role assigned to the function.

**Error 3:**

(custodian) \[ec2-user@ip-172-31-24-18 ~\]$ c7n-org run -c policy/accounts.yml -s output -u policy/ipaddress.yml 2018-06-14 15:04:16,140: c7n\_org:ERROR Exception running policy:invalid-ip-address-login-detected account:sandbox-02 region:us-west-2 error:An error occurred (AccessDeniedException) when calling the CreateFunction operation: Cross-account pass role is not allowed.

This means the role which you have provided is not the correct one.

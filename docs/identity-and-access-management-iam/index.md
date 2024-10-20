---
title: "Identity and Access Management (IAM)"
date: "2018-09-07"
categories: 
  - "devops-tools"
---

## Identity and Access Management (IAM)

 

IAM is an AWS web service which imparts the capability to centrally manage users, credentials and permissions to regulate the user’s access to the AWS resources. With the values like easy use, availability, cost efficiency and many more, AWS has become an indispensable leading infrastructure as a provider.

However, as the organization as well as the need of AWS account grows to include multiple accounts, the added complexities intervenes:

- **Additional Overhead to manage multiple accounts:** For hundreds of thousands of users, creating IAM users, groups, permissions and policies within each account can be a tedious and convoluted process.
- **Additional users and credential management:** Managing users in different accounts with unique long term credentials can be troublesome for the users who need to access different accounts. They would be required to sign-in into every account separately. Nevertheless, managing multiple set of credentials for each account can lead to password fatigue.
- **Tangled Network** to manage users and associated permissions. For instance, revoking myriads of permission access from a former employee can be a rigorous task.
- **Security risks augments with identities in multiple accounts:** With identity threats escalating, accounts become vulnerable to unauthorized access and can lead to security and compliance risks.

**xyz solutions** offers the implementation of a consolidated and holistic scheme to reinforce secure access and visibility, with extended capabilities (e.g. single sign-on (SSO), single master hierarchy, centralized control ,federating existing identity stores and Multi-factor authentication(MFA)) without sacrificing productivity of the organization.

**Basic Schema of Hybrid Cloud Technology**:

A bridge is configured between Microsoft **_Azure Active directory_** (which is the existing identity provider) and Master **_AWS_** account using **_SAML_** (Secured Access Markup Language) and **RBAC** (role-based access control). It alleviates the need to re-create the users in AWS and leverages the already registered users, groups, passwords and roles in Azure to grant controlled privileges to different aspects of AWS, thereby minimizing the administrative overhead. MFA token is allocated on mobile of the user while trying to login through Azure to provide an additional layer of security. **_Cross-Account Manager solution_** of AWS is acquired to create IAM role and delegate permissions to federated users and groups to access multiple AWS accounts with single sign-on capability. It evades security threats as temporary security credentials are generated and automatically rotated.

![](https://cdn-images-1.medium.com/max/1000/1*iE14o6lbNfmdGL8GaMDajw.png)

References:

- [https://aws.amazon.com](https://aws.amazon.com)
- [https://dzone.com](https://dzone.com)

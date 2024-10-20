---
title: "Cloud Custodian"
date: "2018-09-07"
categories: 
  - "devops-tools"
---

## Cloud Custodian: A watchdog guarding the AWS environment

No doubt, AWS is incredible in provisioning us with rich capabilities to construct extensive and convoluted cloud infrastructure, which with Iac can be a matter of few clicks and few hours. However as an old proverb says:

“Nothing comes for free. With great capabilities come great liabilities.”

With the notion to drive the behavior change for companies to start using cloud capabilities without compromising productivity and transparency, along with the ease to create multitude of resources within multiple accounts, it often becomes difficult to manage all these resources and to make sure every bit in your AWS environment is according to the compliance with the standards to maintain security and 100% utilization on cost basis.

“You don’t wish to end up in an environment entwine with mystery what is actually happening into your account.”

Here at [**Devops4solutions**](https://devops4solutions.com) we offer the implementation of Cloud Custodian (a.k.a C7N), as a watchdog to facilitate automated guardrails for account fleet management across all the various applications residing in your account. C7N collates hundreds of defined AWS resources, 300+ filters, 300+ actions combined in arbitrary ways to define policies and provide a unified view of enforcement of policies, with metrics and detailed reporting from a single place. The key aspect is that the policies can run inside the lambda, hence renders real time introspection and action taking capabilities, ensuring compliance compatibility of your accounts state.

![](https://cdn-images-1.medium.com/max/1000/1*QX1rVGZlLTwopYPT66Rqdg.png)

It will keep you secure and will help in saving money, by providing following capabilities but not limited to:

- Will block any S3 Object which is open to public
- Will enable S3 bucket encryption
- Will prohibit any global access to S3
- Will reduce attack window via quick detection and remediation of Security Groups violations
- Will ensure proper configuration of VPC- Flow Log
- Will allow filtering of old EC2 instances and EC2 using unapproved AMIs
- Will detect invalid IP address login
- Will detect Root Logins

“Either notify or take the action automatically, xvz.com will make sure to  guard your account completely.”

References:

- [https://aws.amazon.com](https://aws.amazon.com)
- [https://techbeacon.com](https://techbeacon.com)
- [http://d0.awsstatic.com](http://d0.awsstatic.com)
- [https://www.cloudreach.com](https://www.cloudreach.com)
- [https://dzone.com](https://dzone.com)

---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# An AWS IAM Mistake That Taught Me: Having Permissions Doesn't Always Mean a Service Can Use the Role

This blog shares a practical lesson learned while working with AWS IAM. It explains why an IAM Role with the correct permissions may still fail when used by an AWS service, and highlights the importance of understanding **Trust Policies**, **Permission Policies**, and **iam:PassRole**.

Key topics covered:

* The difference between **Trust Policy** and **Permission Policy** in IAM Roles.
* How Trust Policies determine which AWS service is allowed to assume a role.
* How Permission Policies define what actions the role can perform after being assumed.
* The purpose of **iam:PassRole** and why users configuring AWS services may require this permission.
* Security risks of granting overly broad `iam:PassRole` permissions.
* A practical troubleshooting checklist for diagnosing IAM Role issues:
  * Verify the Trust Policy.
  * Check required permissions.
  * Validate Resource ARNs.
  * Confirm `iam:PassRole` permissions.
  * Review permission boundaries and explicit deny policies.
* AWS IAM security best practices, including the principle of least privilege and the use of temporary credentials instead of long-term access keys.

The article emphasizes that successful IAM configuration requires understanding not only **what permissions a role has**, but also **who is allowed to use the role** and **how AWS services assume that role**.

### Blog Link

https://www.facebook.com/groups/660548818043427/?multi_permalinks=2229747654456861&ref=share

### Reference Materials

* AWS IAM Security Best Practices  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html

* IAM Roles User Guide  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html

* Grant Permission to Pass a Role to an AWS Service  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html

* Policies and Permissions in IAM  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html

* Troubleshoot IAM Roles  
  https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html

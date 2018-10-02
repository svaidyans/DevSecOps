# Managing public cloud Identity and Access Management (IAM) :: Best Practices

## Purpose
To help manage DXCLabs public cloud resources in a more secure way, below are few recommendations and best practices for Identity and Access Management (IAM).

| <a href="Public%20Cloud%20IAM%20best%20practices.md#aws"><img src="images/Icon-AWS.jpg" width="75px" height="75px"> | <a href="Public%20Cloud%20IAM%20best%20practices.md#azure"><img src="images/Icon-Azure.png" width="90px" height="75px"> | <a href="Public%20Cloud%20IAM%20best%20practices.md#ibm-cloud"><img src="images/Icon-IBM Cloud.jpeg" width="90px" height="75px"> | <a href="Public%20Cloud%20IAM%20best%20practices.md#gcp"><img src="images/Icon-GCP.png" width="100px" height="75px"> |
|-----|-------|-----------|-----|

## AWS
- AWS resources are programatically accessed via an access key ID and secret access key.  The access key gives full and unrestricted access to all your account resources and all services. Hence don't create the account access key.  Instead log onto the [AWS Management Console](https://console.aws.amazon.com) with your credentials (userid/password) and create a self IAM user with admin privileges.

- If you do have an access key delete it (AWS Management Console -> Security Credentials -> Access Keys).  If you must have an access key, rotate (change) it regularly.

- [Create individual IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) for people needing access.  Create groups for specific functions with relevant permissions as needed (admin, dev, test, governance, etc.) and add the users to the specific groups.  As much as possible use [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies).  These policies are closely aligned to common [IT functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) and help standardize permissions to services / actions.

- Regularly review and monitor each of your IAM policies. Make sure that your policies grant the [least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) that is needed to perform only the necessary actions.  Set [Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in your policies for accessing resources, like range of IP addresses, using SSL, mandating MFA, etc.

- Use AWS [STS: Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) to create and provide trusted users with temporary security credentials for accessing resources.  These tokens are created dynamically and are short-term, so need not be rotated.  Ansible Tower (v2.4.0 and above) supports STS for accessing EC2 instances.  

- Use [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) that inturn uses STS to manage programmatic access to AWS resources.

- Never share your AWS credentials or keys with anyone and don't embed them in an application/repo.

- Use a [strong password](https://en.wikipedia.org/wiki/Password_strength) to protect access to the AWS Management Console.  Regularly rotate (change) passwords.  Setup a [strong password policy for the IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html) with a requirement to rotate (change) them mandatorily after a certain period of time.

- Enable [MFA: Multi-Factor Authentication](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html).

- Regularly audit and remove unwanted users / roles / policies.  Also delete [unused credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_finding-unused.html).

- Monitor activities:
  - [CloudTrail](https://aws.amazon.com/cloudtrail/) Logs AWS API calls and related events made by or on behalf of an AWS account.
  - [CloudWatch](https://aws.amazon.com/cloudwatch/) Monitors your AWS Cloud resources and the applications you run on AWS. You can set alarms in CloudWatch based on metrics that you define.
  - [Config](https://aws.amazon.com/config/) Provides detailed historical information about the configuration of your AWS resources, including your IAM users, groups, roles, and policies. For example, you can use AWS Config to determine the permissions that belonged to a user or group at a specific time.
  - [S3 Logging](https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html) Logs access requests to your Amazon S3 buckets.
  
- To enhance security, run [Truffle Hog](https://github.com/dxa4481/truffleHog) for ensuring no sensitive info is left unintentionally in any Git repo.
 
[[Goto Top]](#managing-public-cloud-identity-and-access-management-iam--best-practices)

## Azure
-  Apart from the generic security aspects as outlined in the [AWS](Public%20Cloud%20IAM%20best%20practices.md#aws) section above, below are some pointers specific to Azure.

-  DXC has enabled SSO and MFA with Microsoft Azure using the ADFS.  Your application should be configured to use Azure AD as a SAML-based identity provider. As a security control, Azure AD will not issue a token allowing them to sign into the application unless they have been granted access using Azure AD. You may grant access directly, or through a group that they are a member of.  Authentication is explained in detail [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-api-authentication). 

-  [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/vs-secure-secret-appsettings) should be used to securely store and share application code.

-  Azure's built in [RBAC](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles) (Role Based Access Control) can be utilized to assign only the least privilege needed for the user.

-  Use the [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) for managing the resources, groups, policies, etc.

-  [Azure Active Directory Reporting](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-azure-portal) helps to regularly monitor and govern activities.

[[Goto Top]](#managing-public-cloud-identity-and-access-management-iam--best-practices)

## IBM Cloud
-  Apart from the generic security aspects as outlined in the [AWS](Public%20Cloud%20IAM%20best%20practices.md#aws) section above, below are some pointers specific to IBM Cloud.

-  Utilize the IBM Cloud's [App ID](https://console.bluemix.net/docs/services/appid/index.html#gettingstarted) for authentication and authorization. 

-  IBM Cloud has the [Activity Tracker](https://console.bluemix.net/catalog/services/activity-tracker?cm_mmc=IBMBluemixGarageMethod-_-MethodSite-_-10-19-15::12-31-18-_-activity-tracker) to monitor and govern resources.

-  [IBM Cloud Log Analysis](https://console.bluemix.net/docs/services/CloudLogAnalysis/log_analysis_ov.html#log_analysis_ov) is enabled by default and collects / displays logs for your apps, apps runtimes, and compute runtimes where those apps run. 

[[Goto Top]](#managing-public-cloud-identity-and-access-management-iam--best-practices)

## GCP
-  Apart from the generic security aspects as outlined in the [AWS](Public%20Cloud%20IAM%20best%20practices.md#aws) section above, below are some pointers specific to Google Cloud Platform.

-  Utilize the GCP's [Service Account](https://cloud.google.com/iam/docs/understanding-service-accounts) that belongs to an app or VM, rather than individual user, to securely access Google's APIs'.

-  Regulate access control with the help of [Resource Hierarchy](https://cloud.google.com/iam/docs/resource-hierarchy-access-control).

-  Use the [Predefined Roles](https://cloud.google.com/iam/docs/understanding-roles#predefined_roles) for controlling access to resources with the least needed set of privileges.

-  Set [organization-level IAM policies](https://cloud.google.com/resource-manager/docs/access-control-org) to grant access to all projects in your organization.

-  [Audit Logs](https://cloud.google.com/logging/docs/audit/) has Admin Activity and Data Access logs which help regularly audit resources and activities.

[[Goto Top]](#managing-public-cloud-identity-and-access-management-iam--best-practices)

## Feedback
Please share your inputs with [Vaidyanathan Sivasubramanian](mailto:vsivasubram3@csc.com).

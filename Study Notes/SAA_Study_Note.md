# Aws Solutions Architect Associate Note

### Practice Course Link

https://www.udemy.com/course/best-aws-certified-solutions-architect-associate/learn/lecture/29388690#content



### Table of Contents



| No.  | Contents          |      |      |      |      |
| ---- | ----------------- | ---- | ---- | ---- | ---- |
|      | **IAM & AWS CLI** |      |      |      |      |
| 1    | [IAM]()           |      |      |      |      |
| 2    |                   |      |      |      |      |
| 3    |                   |      |      |      |      |

# IAM

= Identity and Access Management

= **Global** service: you can use this console(IAM) in every region.

- **Root**: Default account, shouldn't be used or shared
- **Users**: within your organization, can be grouped
- **Groups**: contain users, not other groups
- Users don't have to be in a group and can be in a multiple groups

## IAM: Permissions

- Users or Groups can be assigned JSON doc called **Policies**
  - define the permissions of the users
- apply the least privilege principle
  - Do not give more permissions than a user actually needs

## IAM: Policies Structure

```
/* An example of using multiple Statements are add in one Policy */
{
  "Version": "2012-10-17", // policy language version, mostly it's “2012–10–17”.
  "Id": "S3 Account-Permissions", // an identifier for the policy
  "Statement": [  //one or more
    {
      "Sid": "FirstStatement", // identifier for the statement
      "Effect": "Allow", // whether the statement allows or denies access (Allow, Deny)
      "Principal": { //account/user/role to which this policy applied to
      	"AWS":["arn:aws:iam::1234:root"]
      }
      "Action": [ //list of actions this policy allows or denies
      	"s3:List*",
        "s3:Get*"
      ], 
      "Resource": [ // resources to which the actions applied to
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ], 
      "Condition": {
      	"Bool": {"aws:MultiFactorAuthPresent": "true"}
      } // for when this policy is in effect.
    }
  ]
}
```

- IAM > Policies : you can edit Policy in JSON format.

## IAM: MFA

- **MFA** (Multi Factor Authentication): password you know + security device you own
  - Password + MFA  => success
- very recommended to use in AWS
-  Users have access to your account and can possibly change configurations or delete resources in your AWS account.
- Protect Root Accounts and IAM users.
- **Benefit** : if a password is stolen or hacked, the account is not compromised.
  - since security device you own should be needed to login
- **Options**: 
  - virtual MFA device: support for multiple tokens(accounts') on a single device.
  - Universal 2nd Factor(U2F) Security Key: Support for multiple root and IAM users, physical device (Proviced by YubiKey by Yubico)
  - Hardware Key Fob MFA device: Proviced by Gemalto
  - Hardware Key Fob MFA device for AWS GovCloud(US): Provided by SurePassID

## IAM: Accesss Options

- 3 options to access AWS
  - AWS Management Comsole (protected by password + MFA)
  - AWS Command Line Interface (CLI): protected by access keys
  - AWS Software Developter Kit (SDK)-for code: protected by access keys
- CLI
  - Access Keys are generated through the AWS Console.
  - Users manage their own access keys
  - Access Keys are secret, just like a password, do not share them

## IAM: CLI

- using commands in your command-line shell
- direct access to the public APIs of AWS services
- you can develop scripts to manage your resources
- alternative to using AWS Management Console.

## IAM: SDK

- Software Development Kit
- Language-specific APIs (set of libraries)
- enables you to access and manage AWS services programmatically
- Embedded within your application
- Supports: SDKS(JavaScript, Python, PHP...), Mobile SDKs(Android, iOS,..), IoT Device SDKs (Embedded C, ...)

## IAM: Role

- not assign to users!!! -> assign permissions to AWS services with IAM roles
- Common roles:
  - EC2 Instance Roles
  - Lambda function roles
  - Roles for CloudFormation

## IAM: Security Tools

- **IAM Credentials Report** (account-level): a report that lists all your account's users and the status of their various credentials.
- **IAM Access Advisor** (User-level): Access advisor shows the service permissions granted to a user and when those services were last accessed

## IAM: Guidelines

- one physical user = one AWS user
- Assign users to groups and assign permissions to groups
- create a strong password policy
- use and enforce the use of Multi Factor Authentication
- create and use roles for giving permissions to AWS services
- use Access Keys for Programmatic Access
- Audit permissions of your account using IAM credentials Report & IAM access Advisor

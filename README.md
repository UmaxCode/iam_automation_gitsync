# IAM Automation CloudFormation Template

## Overview

This CloudFormation template automates the creation of IAM resources, including users, groups, policies, and integrates with AWS Secrets Manager and EventBridge. The template includes automation for generating one-time passwords for IAM users, assigning them to appropriate IAM groups, and providing email addresses for each user through SSM Parameters. Additionally, it deploys an EventBridge rule and Lambda function that logs user credentials when an IAM user is created.

## Purpose of GitSync

**GitSync** is used in this project to automate the deployment of CloudFormation templates directly from a Git repository. It enables you to manage CloudFormation templates in version control and ensure that your AWS resources are deployed in a consistent, repeatable manner. By using GitSync:

- You can **automate the synchronization** of CloudFormation stack deployments every time the template is updated in the repository.
- It simplifies **deployment pipelines**, especially when integrating with tools like AWS CodePipeline, or manually via GitSync integration with AWS services.
- You ensure that any changes made to the CloudFormation template are **version-controlled** and can be easily rolled back or audited.
  
In this case, GitSync allows you to **automatically trigger updates** to your AWS infrastructure based on changes made to the CloudFormation template stored in your Git repository.

## Features

- **IAM User Creation**: Automatically creates two IAM users (`user1` and `user2`) with specific groups.
- **One-Time Password Generation**: Uses AWS Secrets Manager to generate and store a one-time password for each user.
- **S3 Access Policy**: Grants read access to S3 for users in the relevant IAM groups.
- **Email Parameter Storage**: Stores email addresses for each user in AWS Systems Manager (SSM) Parameter Store.
- **Lambda Function**: Logs IAM user credentials (username, email, and password) when a new user is created.
- **EventBridge Integration**: Automatically triggers the Lambda function on the "CreateUser" event from IAM, allowing for real-time processing.

## Template Breakdown

The CloudFormation template includes the following resources:

- **Secrets Manager**: Creates a secret (`MySecretForIAMUsers`) for storing one-time passwords.
- **IAM Groups**: Creates two IAM groups with S3 read access.
- **IAM Policies**: Grants S3 read access to users in the IAM groups.
- **IAM Users**: Creates users (`user1` and `user2`) with their respective email addresses stored in SSM.
- **Lambda Function**: A function (`PrintUsersCredentials`) that retrieves and logs user details.
- **EventBridge Rule**: A rule to capture IAM user creation events and invoke the Lambda function.

## Template Parameters

The template requires the following parameters to be provided during stack creation:

- `user1Name`: The username for the first IAM user.
- `user1Email`: The email address for the first IAM user.
- `user2Name`: The username for the second IAM user.
- `user2Email`: The email address for the second IAM user.


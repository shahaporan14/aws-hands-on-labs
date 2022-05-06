# Create User using AWS Management Console

## Step-01: Account Alias
  - Click on the Edit
    - Preferred alias : own-dark-practice 
    - Description:  Alias need to be globally unique and we can generate user friendly name which would be the url for sigin in 

## Step-02: Create User 
  - Click on the Users-> Add Users
  - Name: Admin
  - Select AWS access type:
      - Programmatic access : YES (Checked) 
          - Description: This Enables an access key ID and secret access key for the AWS API, CLI, SDK, and other development tools.
      - Password AWS Management Console access: Yes (Checked) 
          - Description: Enables a password that allows users to sign-in to the AWS Management Console
  - Require password reset: NO (Unchecked) 
    - Description: whenever you will login and you must create a new password at next sign-in if you check password reset. 
        Otherwise , you don't need to reset your password because it's unchecked
  - Create Group:
      - Name: AdminGroup
      - Policy: AdministratorAccess 
        - Description: This Policy will provide significate Access for this group which can assign for any user
## Step-03: Verifiy AWS Console
  - Console URL: https://own-dark-practice.signin.aws.amazon.com/console
    -  Name: Admin
    - pasword: ******

## Step-04: Configure With AWS CLI
  - aws configure --profile Admin
  - AWS Access Key ID [None]: YOUR ACCESS KEYID
  - AWS Secret Access Key [None]: YOUR SECRET ACCESS KEY
  - Default region name [None]:
  - Default output format [None]:

## Step-04: Verify using AWS CLI
  - aws s3 ls --profile admin



# cloudfixLinter-demo-cloudformation

## Steps on running the demo

### Prelimnaries

1. This repo has 2 Cf templates that  will create 23 resources and 5 resources repectively:-

#### Cf-Template : all-resources-template

| Resource Type   |      Count      |
|:----------:|:-------------:|
| AWS::DynamoDB::Table |  1 |   
| AWS::EC2::Volume     |  1 |
| AWS::EC2::SecurityGroup |  4 |
| AWS::IAM::Role       |  2 |
| AWS::EC2::Instance   |  2 |
| AWS::IAM::InstanceProfile |  2 |
| AWS::EC2::VPCEndpoint|  1 | 
| AWS::S3::Bucket      |  2 |
| AWS::EFS::MountTarget|  1 | 
| AWS::EFS::FileSystem |  1 |
| AWS::Neptune::DBSubnetGroup |  1 |
| AWS::EC2::SecurityGroup  |  1 |
| AWS::Neptune::DBInstance |  1 | 
| AWS::Neptune::DBCluster |  1 |
| AWS::EC2::NatGateway |  1 | 



## Using Extension with Cloudformation

 ### 1.  AWS Creds setup
  - Login to AWS using terminal (in default profile) by any of the following options:
    - `aws configure` to login with permanent credentials (using `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
    - To login with temporary credentials (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, `AWS_SECURITY_TOKEN`), follow [this](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
    - Using `saml2aws`. For user guide visit [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
      or 
   - Steps to setup saml2aws for Trilogy account holders -
      1.  run `saml2aws configure`
      2.  choose `KeyCloak` as service provider
      3.  Provide this url - https://devfactory.devconnect-df.com/auth/realms/devfactory/protocol/saml/clients/aws (url works for trilogy account holders only)
      4.  Enter your AD detials.     
      
    - Note: Setting `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` as enviroment variables from terminal won't work because they are just available in the terminal instance in which have set them and not available globally.

 ### 2.  Region set up   
   - The region for default profile should also be set to the region where the stack exists. Use the command `aws configure`. Following is an example of setting the region to `us-east-1`
      ```
      AWS Access Key ID [****************H44M]: 
      AWS Secret Access Key [****************9jFj]: 
      Default region name [None]: us-east-1
      Default output format [None]:
      ```
### 3.  Setup SubnetId's for deploying resources    
   - Before deploying your stack, you need to edit the `SubnetId` [here](./Templates1/cf-template.json#L22) for `cloudfix-linter-demo-cloudformation` stack and [all subnets in this file](https://github.com/trilogy-group/cloudfixLinter-demo-cloudformation/blob/newResources/README.md) for `CfDemoStack` stack to the id of a subnet that exists in your account and to which you would like to deploy the resources.

 ### 4. Stack Deployment    
**Note : - For demo purpose we have already deployed a stack with name `CfDemoStack` for using `all-resources-template.json` and `cloudfix-linter-demo-cloudformation` for using `cf-template.json` with given template in Q3 of TrilogyAccount. You can use it for demo purpose, if you are a Trilogy account holder.**       
   - Deploy a stack in your AWS account using the template (No need to do this again if deployed once and that stack isn't deleted). There are two ways to do this:
    - Use AWS [console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks)

    - Login to aws through CLI, then run this command
      ```
      aws cloudformation deploy --template-file Templates1/all-resources-template.json --stack-name <stackName> --capabilities CAPABILITY_IAM
      ```
   - For testing on lesser resources please use cf-template.json instead of all-resources-template

 ***Note : Stack creates IAM roles. Please ensure you have sufficient permissions to create IAM roles**
 
 ### 5.  Fetching recommendations    
   - For viewing **mock recommendations**. Since Cloudfix won't give recommendations for your resources till it has observed behaviour for 14 days, so for a quick preview of the tool mock recommendations can be used to fake 
   #### To use mock recommendations.
   In order to generate mock recommnedations and tell the linter that it needs to read reccomendations from a file rather than from CloudFix itself, on the terminal run
   - Windows
   ```
   $env:CLOUDFIX_FILE=$true
   python utils/gen_recco.py <stackName>
   ```
   - Linux and Devspaces
   ```
   export CLOUDFIX_FILE=true
   python utils/gen_recco.py <stackName>
   ```


  - This will generate a reccos.json file which will be used to facilitate mock recommendations     
    
   #### To get cloudFix recommendations via CLI

   - Windows
   ```
   $env:CLOUDFIX_FILE=$false
   $env:CLOUDFIX_USERNAME="<MY_USERNAME>"
   $env:CLOUDFIX_PASSWORD="<PASSWORD>"
   ```
   - Linux and Devspaces
   ```
   export CLOUDFIX_FILE=false
   export CLOUDFIX_USERNAME="<MY_USERNAME>"
   export CLOUDFIX_PASSWORD="<PASSWORD>"
  
   ```            
  - Note: For this step we use AWS SDK to fetch stacks details from your AWS account

 ### 6. Open command pallete    
   - Press `Ctrl+Shift+p` to open VS Code command palette

 ### 7. Cloudfix-linter extension initialization    
   - Run `Cloudfix-linter: init` command   
    1. Choose `mock-recommendations` for mock recommendation    
    2. Choose `CloudFix-service-recommendations` for Cloudfix API recommendations
       - Provide Cloudfix account credentials
 ### 8. Saving the cf templates    
   - Save/Edit cloudformation template file to view recommendations

Note: If you use the given template to deploy multiple stacks(in the same env -> Account+Region), recommendations from all the stacks will be linted along with the stack name for the recommendation

## Steps to add cloudfix-linter in your $PATH env variable
 ### On MacOS 
  - For Setting the path to cloudfix-linter binary in your current terminal only - 
    - run `export PATH=$PATH:~/.cloudfix-linter/bin/`
  - For setting path to cloudfix-linter binary across your system - 
    - run `sudo nano /etc/paths`
    - provide password
    - Add the following line at the end of the file: `/Users/your_username/.cloudfix-linter/bin`    
    (Note: Replace your_username with your actual username.)
    - Save the file by pressing Control+X, then Y, then Enter.
    - Close the Terminal and reopen it for the changes to take effect.

  - run `cloudfix-linter -v ` to check path has been set successfully

 ### On Linux
  - run `nano ~/.bashrc`        
  - Add export PATH="~/.cloudfix-linter/bin:$PATH" to the last line of the file, where your-dir is the directory you want to add.     
  - Save the .bashrc file.     
  - Restart your terminal.  
  - run `cloudfix-linter -v ` to check path has been set successfully    

### FAQ

1. Not able to deploy the stack in aws account
 - Please check permissions authorized to your aws account. Since provided stack creates IAM Roles also, ensure you have sufficient permissions.

2. Running `python utils/gen_recco.py <stackName>` gives - An error occurred (ExpiredToken) when calling the DescribeStackResources operation: The security token included in the request is expired
 - To fix this issue, please refer to the usage guide and make sure that you're providing the correct AWS credentials using one of the recommended methods

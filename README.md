# cloudfixLinter-demo-cloudformation

## Using Extension with Cloudformation
 1. Login to AWS using terminal (in default profile) by any of the following options:
    - `aws configure` to login with permanent credentials (using `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
    - To login with temporary credentials (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, `AWS_SECURITY_TOKEN`), follow [this](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
    - Using `saml2aws`. For user guide visit [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
    - Note: Setting `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` as enviroment variables from terminal won't work because they are just available in the terminal instance you set them into and not available globally.
 2. Deploy a stack in your AWS account using the template (No need to do this again if done once and that stack isn't deleted). There are two ways to do this:
    1. Use AWS [console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks)
    2. Login to aws through CLI, then run this command
    ```
    aws cloudformation deploy --template-file Templates1/cf-template.json --stack-name <stackName>
    ```
 3. The region for default profile should also be set to the region where the stack exists. Use the command `aws configure`. Following is an example of setting the region to us-east-1
 ```
 AWS Access Key ID [****************H44M]: 
 AWS Secret Access Key [****************9jFj]: 
 Default region name [None]: us-east-1
 Default output format [None]:
 ```
 3. For mock:
    - Run command `python utils/gen_recco.py <stackName>` 
    - This will generate a reccos.json file which will be used to facilitate mock recommendations
    - Note: For this step also uses AWS SDK to fetch stacks details from your AWS account
 4. Press `Ctrl+Shift+p` to open VS Code command palette
 5. Run `Cloudfix-linter: init` command
    1. Choose `mock-recommendations` for mock recommendation
    2. Choose `CloudFix-service-recommendations` for Cloudfix API recommendations
       - Provide Cloudfix account credentials
 6. Save/Edit cloudformation template file to view recommendations

Note: If you use the given template to deploy multiple stacks(in the same env -> Account+Region), recommendations from all the stacks will be linted along with the stack name for the recommendation
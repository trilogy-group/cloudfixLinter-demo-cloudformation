# cloudfixLinter-demo-cloudformation

## Using Extension with Cloudformation
 1. Login to AWS using terminal (in default profile) by any of the following options:
    - `aws configure` to login with permanent credentials (using `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
    - To login with temporary credentials (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, `AWS_SECURITY_TOKEN`), follow [this](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
    - Using `saml2aws`. For user guide visit [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli)
    - Note: Setting `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` as enviroment variables from terminal won't work because they are just available in the terminal instance you set them into and not available globally.
 2. Before deploying your stack, you need to edit the `SubnetId` [here](./Templates1/cf-template.json#L22)
 3. Deploy a stack in your AWS account using the template (No need to do this again if deployed once and that stack isn't deleted). There are two ways to do this:
    - Use AWS [console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks)
    - Login to aws through CLI, then run this command
      ```
      aws cloudformation deploy --template-file Templates1/cf-template.json --stack-name <stackName>
      ```
 4. The region for default profile should also be set to the region where the stack exists. Use the command `aws configure`. Following is an example of setting the region to us-east-1
      ```
      AWS Access Key ID [****************H44M]: 
      AWS Secret Access Key [****************9jFj]: 
      Default region name [None]: us-east-1
      Default output format [None]:
      ```
 5. For viewing **mock recommendations**. Since Cloudfix won't give recommendations for your resources till it has observed behaviour for 14 days, so for a quick preview of the tool mock recommendations can be used to fake Cloudfix recommendations for your resources:
    - Run command `python utils/gen_recco.py <stackName>` 
    - This will generate a reccos.json file which will be used to facilitate mock recommendations
    - Note: For this step we use AWS SDK to fetch stacks details from your AWS account
 6. Press `Ctrl+Shift+p` to open VS Code command palette
 7. Run `Cloudfix-linter: init` command
    1. Choose `mock-recommendations` for mock recommendation
    2. Choose `CloudFix-service-recommendations` for Cloudfix API recommendations
       - Provide Cloudfix account credentials
 8. Save/Edit cloudformation template file to view recommendations

Note: If you use the given template to deploy multiple stacks(in the same env -> Account+Region), recommendations from all the stacks will be linted along with the stack name for the recommendation
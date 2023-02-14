# cloudfixLinter-demo-cloudformation

## Using Extension with Cloudformation
 1. Deploy a stack in your AWS account using the template (No need to do this again if done once and that stack isn't deleted). There are two ways to do this:
    1. Use AWS [console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks)
    2. Login to aws through CLI, then run this command 
 2. Login to AWS using terminal by any of the AWS login methods:
    1. `aws configure` to setup your AWS account (in default profile)
    2. Using `saml2aws`
    3. Set and export `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` as enviroment variables.
 3. The region for default profile should also be set to the region where the stack exists
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

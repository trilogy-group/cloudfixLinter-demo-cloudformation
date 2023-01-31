# cloudfixLinter-demo-cloudformation

##### PREREQUISITE
user must have cfn-lint installed in his system to do testing on local.  
For MacOS you can install cfn-lint using [`brew install cfn-lint`](https://formulae.brew.sh/formula/cfn-lint).
For Windows and linux [`pip install cfn-lint`](https://pypi.org/project/cfn-lint/)
For Local Testing 
 1. Create the go binaries from cloudfix-linter repo
 2. Move the binary and mynewrule.py in current current directory inside cloudfix-linter folder
 4. Run `export CLOUDFIX_FILE=true` for mac/linux. For windows run `$env:CLOUDFIX_FILE=$true`. 
 3. Run `./cloudfix-linter/cloudfix-linter-cloudformation recco` to get the reccomendations.

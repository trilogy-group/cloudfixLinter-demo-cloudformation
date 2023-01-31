# cloudfixLinter-demo-cloudformation

For Local Testing 
 1. Create the go binaries from cloudfix-linter repo
 2. Move the binary and mynewrule.py in current current directory inside cloudfix-linter folder
 4. Run `export CLOUDFIX_FILE=true` for mac/linux. For windows run `$env:CLOUDFIX_FILE=$true`. 
 3. Run `./cloudfix-linter/cloudfix-linter-cloudformation recco` to get the reccomendations.

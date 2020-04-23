# Summary
Cloudformation template that creates a codepipeline

# to create codepipeline
```
      aws cloudformation create-stack --stack-name cpStack --template-body file://./codepipeline_template.yml \
        --capabilities CAPABILITY_IAM \
        --parameters \
        ParameterKey=GitHubUser,ParameterValue=xxx \
        ParameterKey=GitHubRepo,ParameterValue=xxx \
        ParameterKey=GitHubBranch,ParameterValue=xxx \
        ParameterKey=GitHubToken,ParameterValue=xxx
```
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

# Summary
Cloudformation template that creates an AWS codepipeline. The pipeline gets its source from a github repo, builds and tests using an AWS codebuild project, and deploys to an Elastic Beanstalk environment.

# Create the codepipeline
```
    aws cloudformation deploy --stack-name <stack name> --template-file codepipeline_template.yml \
        --capabilities CAPABILITY_IAM \
        --parameter-overrides \
        GitHubUser=<user> \
        GitHubRepo=<repo> \
        GitHubBranch=<branch> \
        GitHubToken=<personal token> \
        ElasticBeanstalkApplication="application ID" \
        ElasticBeanstalkEnvironment="environment ID" \
        CloudWatchLogGroup=<codebuild log group> \
        CloudWatchLogStream=<codebuild log stream>
```

# Test deployed application
After pipeline creation, there will be an initial release. Open the URL for the Elastic Beanstalk environment to verify that the application is running.

# Update the application
Push a change to the application repo branch, the codepipeline should execute.


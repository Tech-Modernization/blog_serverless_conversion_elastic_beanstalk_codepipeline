# Summary
This repo include the followings:
* A cloudformation template that creates an pair of Elastic Beanstalk application and environment. The pair is to be used as the deployment target of a codepipeline.
* A Cloudformation template that creates an AWS codepipeline. The pipeline gets its source from a github repo, builds and tests using an AWS codebuild project, and deploys the application to an Elastic Beanstalk environment.

# Features of the Elastic Beanstalk environment:
* An ELB fronts an autoscaling group.
* The ELB logs access to an S3 bucket.
* The TargetGroup response time triggers scaling action of the auto scaling group.
* Instance logs are streams to CloudWatch log.

# Features of the codepipeline:
* Test the application in a codebuild container.
* Deploy application only tests passes.

# to create the elastic beanstalk application and environment, and get the application and environment Ids
```
aws cloudformation deploy --stack-name <elastic beanstalk stack name> --template-file elasticBeanStalk_setup.yml \
        --capabilities CAPABILITY_IAM \
        --parameter-overrides \
        SolutionStackName="64bit Amazon Linux 2018.03 v4.14.1 running Node.js" \
        NotificationEmail=<youremail@yourcom.com> \
        EC2KeyName=<ec2_sshkeypair_name>

aws cloudformation describe-stacks --stack-name <stack_name> | jq ".Stacks[0].Outputs[]"
```

# to create the codepipeline
```
aws cloudformation deploy --stack-name <codepipeline stack name> --template-file codepipeline_template.yml \
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

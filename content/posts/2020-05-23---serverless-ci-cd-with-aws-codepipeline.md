---
title: Serverless CI/CD with AWS CodePipeline
date: "2020-05-23T00:00:00.000Z"
template: "post"
category: "Development"
tags: 
  - "Cloud"
  - "AWS"
  - "Serverless"
draft: false
slug: "/posts/deploy-service-fabric-type-when-one-of-its-services-needs-deletion/"
description: "Sometimes, you create a service but your are not happy with the naming. After fierce debate with colleagues, you rename the service. Locally it runs ok and everything looks fine. Yet the renaming the service can bite you back when you deploy it to a remote Service Fabric cluster via Azure Devops."
socialImage: "/media/42-line-bible.jpg"
---

I've set up a CI/CD Pipeline for my serverless project. This is my settings.

### Create a new pipeline

* name: navien-installer-infrastructure
* Service role: New service role
* Role name: automatically populated

### Add source stage

* Provider: Github
* Use "Connect to Github"
* Change detection options: Github webhooks \(recommended\)
* branch: refs/head/master

You can specify tag, branch, or pull request. I simply put branch name, as I want this to run when it gets merged into master branch. For local development, I would run "sls deploy" manually

### Add build stage

* Provider: AWS CodeBuild
* Region: Europe \(Ireland\)
* Create a new project

If you haven't created a project, create one now

### Project

* Name: navien-installer-infrastructure
* Environment image: Managed image
* Operating system: Ubuntu
* Runtime: Standard
* Image: aws/codebuild/standard:4.0
* Image version: Always use the latest
* Environment type: Linux
* Service role: New service role

#### Role

Initially, I encountered an error because the role this pipeline uses doesn't have enough permission to create a stack. To enable serverless framework to create a new stack, I have the below permissions. 

* AWSLambdaFullAccess
* IAMFullAccess
* AmazonAPIGatewayAdministrator
* CloudFormationAdministrator

#### Buildspec

Buildsec is a yaml file that speifies build steps. 

```yaml
version: 0.2
env:
  variables:
    DOTNET_ROOT: /root/.dotnet
phases:
  install:
    runtime-versions:
      dotnet: 2.1
  pre_build:
    commands:
      - npm install -g serverless
      - cd src
      - cd Navien.Installers.Lambdas
      - dotnet clean
      - dotnet restore
  build:
    commands:
      - ./build.sh
      - serverless deploy --stage cicd | tee deploy.out
```

### Add deploy stage

I'v skipped this stage as "serverless deploy" will do the deployment.

### Review

Review and "Create pipeline"

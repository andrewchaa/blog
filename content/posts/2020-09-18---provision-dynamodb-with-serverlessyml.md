---
title: Provision DynamoDb with serverless.yml
date: "2020-09-18T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-09-18/provision-dynamodb-with-serverlessyml/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

Basically, it employs the same syntax as Cloud Formation json but is in yaml. 

More details to here: [https://www.serverless.com/dynamodb](https://www.serverless.com/dynamodb) 

```yaml
resources:
  Resources:
    companies:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: companies
        AttributeDefinitions:
          - AttributeName: gasSafeNumber
            AttributeType: S
        KeySchema:
          - AttributeName: gasSafeNumber
            KeyType: HASH
        ProvisionedThroughput:
          ReadCapacityUnits: 3
          WriteCapacityUnits: 3
```




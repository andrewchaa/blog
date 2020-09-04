---
title: Adding a custom endpoint to AWS Amplify API
date: "2020-09-02T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-09-02/adding-a-custom-endpoint-to-aws-amplify-api/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

I have a project that I've used amplify on javascript and serverless framwork on C\#. To use the both APIs, I need a way to call the endpoint fro Amplify's API object. 

I've added the existing custom api endpoint to the config file

```javascript
"aws_cloud_logic_custom": [
    {
        "name": "new_api",
        "endpoint": "https://xxxxxx1.execute-api.eu-west-1.amazonaws.com/dev",
        "region": "eu-west-1"
    },
    {
        "name": "existing_api",
        "endpoint": "https://xxxxxx2.execute-api.eu-west-1.amazonaws.com/dev",
        "region": "eu-west-1"
    }
],
```

One thing to be careful about is you can't have the name, `aws-exports.js`. It's the default config file name and the content of the file will be overwritten on Amplify Console build. I named mine to `aws-exports-custom.js`.  


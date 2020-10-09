---
title: Prettify JSON with C\#
date: "2020-10-05T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-10-05/prettify-json-with-c/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

Formating JSON is easy peasy with javascript. There are so many different  flavours of libraries. Yet, it's not true with C\#. The other day, I encountered a use case where I had to prettify JSON in C\#.

Interestingly, [JSON.Net](https://www.newtonsoft.com/json) supported the feature already. The following's the code.

```csharp
var content = Encoding.UTF8.GetString(message.Body);
var beautified = JToken.Parse(content).ToString(Formatting.Indented);
```


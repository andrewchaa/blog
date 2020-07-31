---
title: Fail the deserializatioin of JSON string if any expected properties don't exist
date: "2020-07-23T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-07-23/fail-the-deserializatioin-of-json-string-if-any-expected-properties-dont-exist/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

I can simply put \[Required\] attribute in ASP.NET Core. On my personal project, I use AWS Lambda and the SDK doesn't have that feature out of the box. 

Json.NET has another handy attribute, \[JsonObject\(ItemRequired = Required.Always\)\]. It makes every property of the class mandatory

```csharp
[JsonObject(ItemRequired = Required.Always)]
public class UpdateWarrantyRequest
{
    public int WarrantyYear { get; set; }
}
```

Then I can catch the exception and handle it.

```csharp
public static class JsonExtensions
{
    public static OperationResult<T> To<T>(this string payload)
    {
        try
        {
            return new OperationResult<T>(HttpStatusCode.OK, 
                JsonConvert.DeserializeObject<T>(payload));
        }
        catch (Exception)
        {
            var errorMessage = $"Failed in deserializing the payload:\n {payload}";
            Console.WriteLine(errorMessage);
            return new OperationResult<T>(HttpStatusCode.BadRequest, errorMessage);
        }
    }
}

```


---
title: Creting a test Subscription to Azure Service Bus Topic
date: "2020-08-25T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-08-25/creting-a-test-subscription-to-azure-service-bus-topic/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

You run Integration tests for your service. In my case, I have a local Cosmos DB emulator and local SQL Server instance. So the Integrationt tests run by calling APIs on the local host, storing the data into a local Cosmos DB. However, the messaging is tricky. It still sends messages to the Service Bus on the Test environment. The problem is, then, there's another instance running on the Test environment. Two instances compete for the same message. When the message is successfully processed, it's not clear which instance processed the message. The instance on the Test environment will affect the test result of the Integratino tests. 

To solve the issue, I create another subscription, "\*-test". It'll be only created when the service runs in Debug mode on a local development machine. 

```csharp
public static void ConfigureServices(HostBuilderContext context, IServiceCollection services)
{
    ....
#if DEBUG
    var client = new ManagementClient(serviceBusOptions.ConnectionString);
    foreach (var topic in new[]
    {
        EventNames.TransactionCreated,
    })
    {
        if (!client.SubscriptionExistsAsync(topic, EventNames.Subscripton).GetAwaiter().GetResult())
        {
            client.CreateSubscriptionAsync(new SubscriptionDescription(topic,
                EventNames.Subscripton));
        }
    }
#endif
    ....    

```


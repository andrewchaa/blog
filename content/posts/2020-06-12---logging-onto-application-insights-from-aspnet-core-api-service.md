---
title: Logging onto Application Insights from ASP.NET Core api service
date: "2020-06-12T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/logging-onto-application-insights-from-aspnet-core-api-service/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

You can send trace logs to Application Insight.

Install Microsoft.ApplicationInsights.AspNetCore package

```bash
Microsoft.ApplicationInsights.AspNetCore
```

Add Application Insights telemetery in ConfigureServices

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.AddApplicationInsightsTelemetry();
}
```

Set up the instrumentation key in your appsettings.json

```javascript
{
  "ApplicationInsights": {
    "InstrumentationKey": "#{app_insights_instrumentation_key}#"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

For some reason, ApplicationInsightsTelemetry setting doesn't resepct the default LogLevel. You have to set it manually by configuring the logging

```csharp
return new WebHostBuilder()
    .UseKestrel()
    .ConfigureServices(
        services => services
            .AddSingleton<StatelessServiceContext>(serviceContext))
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
    .UseUrls(url)
    .ConfigureLogging(l =>
    {
        l.AddConsole();
        l.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Information);
    })
    .Build();
```

Now, ILogger is plumbed into Application Insights' traces logging.

```csharp
public TransactionsController(ILogger<TransactionsController> logger)
{
    _logger = logger;
}

public async Task<IActionResult> Create([FromRoute]Guid transactionid, 
    [FromBody]CreateTransactionRequest request)
{
    _logger.LogInformation($"Creating a transaction: {transactionid}");
```


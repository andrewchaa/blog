---
title: Typed Configuration in ASP.NET Core
date: "2020-09-10T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-09-10/typed-configuration-in-aspnet-core/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

Another great thing I like is Typed Configuration in .NET Core. You create a class and bind the configuration section to it. It was possible before but it is much easier now.

```csharp
public class KeyVaultOptions
{
    public string TenantId { get; set; }
    public string ClientId { get; set; }
    public string ClientSecret { get; set; }
}

```

Then bind the DTO class to the config section.

```csharp
private static IServiceCollection AddConfigurations(this IServiceCollection services,
    IConfiguration configuration)
{
    services.Configure<KeyVaultOptions>(configuration.GetSection("KeyVault"));
    return services;
}

```


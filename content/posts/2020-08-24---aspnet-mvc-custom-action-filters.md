---
title: ASP.NET MVC Custom Action Filters
date: "2020-08-24T00:00:00.000Z"
template: "post"
category: "Development"
tags:
draft: false
slug: "/posts/2020-08-24/aspnet-mvc-custom-action-filters/"
description: ""
socialImage: "/media/42-line-bible.jpg"
---
  

it's been a while since I worked on ASP.NET MVC web. For the last two years, I've mostly worked on APIs on the server-side and on React on the client-side, luckily. Now it's time to bite the bullet and do some work on one of the legacy apps. 

We need to log all user activities for enhanced security monitoring. in ASP.NET core, I would be using a custom middleware. A similar thing for MVC is Action Filter. By adding a [custom Action Filter](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters) to the global Filter collection, I would be able to log all request related information.

Btw, here Filter means the filter in [pipeline and filter design pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters).

### Create a filter

```csharp
public class RequestLoggingFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext filterContext)
    {
        System.Diagnostics.Trace.TraceInformation($"Url: {filterContext.HttpContext.Request.Url}");
        System.Diagnostics.Trace.TraceInformation($"UserAgent: {filterContext.HttpContext.Request.UserAgent}");
        System.Diagnostics.Trace.TraceInformation($"Client IP Address: {filterContext.HttpContext.Request.UserHostAddress}");

        var user = filterContext.HttpContext.User as MvcPrincipal;
        if (user == null)
            return;

        System.Diagnostics.Trace.TraceInformation($"Authenticationid: {user.AuthenticationId}");
        System.Diagnostics.Trace.TraceInformation($"CollectiveApprovalId: {user.CollectiveApprovalUserId}");
        System.Diagnostics.Trace.TraceInformation($"InstitutionId: {user.InstitutionId}");
        System.Diagnostics.Trace.TraceInformation($"Organisation: {user.Organisation}");
        System.Diagnostics.Trace.TraceInformation($"PrincipalId: {user.PrincipalId}");
        System.Diagnostics.Trace.TraceInformation($"User Name: {user.DisplayName}");
        System.Diagnostics.Trace.TraceInformation($"User Email: {user.Email}");

        System.Diagnostics.Trace.TraceInformation($"User Roles: {string.Join(", ", user.PrincipalClaims)}");
    }

    public void OnActionExecuted(ActionExecutedContext filterContext) { }
}

```

### Register the filter to the Global Filter collection

Once you have your custom Filter, you need to register it, so that the pipeline execute your fiter.

```csharp
public class FilterConfig
{
    public static void RegisterGlobalFilters(GlobalFilterCollection filters)
    {
        ....
        filters.Add(new RequestLoggingFilter());
    }
}


public class MvcApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        ....

        FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    }
```


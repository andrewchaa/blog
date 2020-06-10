---
title: Visual Studio sln content for Service Fabric service
date: "2020-06-10T00:00:00.000Z"
template: "post"
category: "Development"
tags: 
  - "Visual Studio"
draft: false
slug: "/posts/visual-studio-sln-content-for-service-fabric-service/"
description: "Service Fabric application only runs on x64 platform. By default, Visual Studio creates quite a few  different combination of settings"
socialImage: "/media/42-line-bible.jpg"
---

Service Fabric application only runs on x64 platform. By default, Visual Studio creates quite a few  different combination of settings

* Any CPU.ActiveCfg = Debug\|Any CPU
* Any CPU.ActiveCfg = Debug\|x64
* Any CPU.ActiveCfg = Release\|x64
* x64.ActiveCfg = Debug\|x64
* x64.Build.0 = Debug\|x64
* x64.Deploy.0 = Debug\|x64

```text
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|Any CPU.ActiveCfg = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.ActiveCfg = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.Build.0 = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.Deploy.0 = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|Any CPU.ActiveCfg = Release|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.ActiveCfg = Release|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.Build.0 = Release|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.Deploy.0 = Release|x64
```

You don't need all of them. Especially those with Any CPU. Each project have 8 different settings. What you need is just 4 settings for each project. 

```text
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.ActiveCfg = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.Build.0 = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.ActiveCfg = Release|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.Build.0 = Release|x64

# the below two are only for service fabric
{27F34332-1E6A-4228-9D54-F79324D9011B}.Debug|x64.Deploy.0 = Debug|x64
{27F34332-1E6A-4228-9D54-F79324D9011B}.Release|x64.Deploy.0 = Release|x64
```




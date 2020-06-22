---
title: Fix ASP.NET Core port number on Service Fabric
date: "2019-02-06T00:00:00.000Z"
template: "post"
category: "Programming"
tags:
  - "ASP.NET Core"
draft: false
slug: "/posts/fix-asp-net-core-on-port-number-on-service-fabric/"
description: "If you create a stateless asp.net core api as service fabric service, the port number changes each time you restart the service fabric service."
socialImage: "/media/42-line-bible.jpg"

---

If you create a stateless asp.net core api as service fabric service, the port number changes each time you restart the service fabric service.
It's quite annoying that your postman requests start failing because the port number has changed. To fix it, [you have to override the resource, specifying the port number](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-how-to-specify-port-number-using-parameters).

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="8001"/>
      </Endpoints>
    </ResourceOverrides>
    <ConfigOverrides />
</ServiceManifestImport>
```


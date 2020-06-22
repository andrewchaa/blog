---
title: Calling api endpoints with Refit
date: "2020-06-12T00:00:00.000Z"
template: "post"
category: "Development"
tags: 
  - "API"
  - "Test"
  - "Refit"
draft: false
slug: "/posts/calling-api-endpoints-with-refit/"
description: "Refit is the automatic type-safe REST library for .NET Core, Xamarin and .NET. Heavily inspired by Square's Retrofit library"
socialImage: "/media/42-line-bible.jpg"
---

[Refit ](https://github.com/reactiveui/refit)is

> The automatic type-safe REST library for .NET Core, Xamarin and .NET. Heavily inspired by Square's Retrofit library, Refit turns your REST API into a live interface

Refit turns your REST API into a live interface

```csharp
public interface IApi
{
    [Put("/v1/transactions/{transactionId}")]
    Task<HttpResponseMessage> CreateTransaction(Guid transactionId);
}
```

You can expose your test client with Fixture in xUnit.

```csharp
public TestClientFixture()
{
    Api = RestService.For<IApi>("http://localhost:8200");
}
```

In the test file, you call the actual api

```csharp
public CreateTransactionTests(TestClientFixture clientFixture, 
    ITestOutputHelper output)
{
    _output = output;
    _api = clientFixture.Api;
    _fixture = new Fixture();
}

[Fact]
public async Task Should_return_200_Ok()
{
    var transactionId = Guid.NewGuid();
    var request = _fixture.Build<CreateTransactionRequest>()
        .With(x => x.Currency, "EUR")
        .Create();
    var response = await _api.CreateTransaction(transactionId, request);
    _output.WriteLine(await response.Content.ReadAsStringAsync());

    Assert.True(response.IsSuccessStatusCode);
}

```


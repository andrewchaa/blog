---
title: Writing up dependency injection container for your component tests
date: "2019-11-13T00:00:00.000Z"
template: "post"
category: "Programming"
tags: 
  - "Testing"
  - "Component Testing"
draft: false
slug: "/posts/dotnet-dependency-container-for-component-tests/"
description: ""
socialImage: "/media/42-line-bible.jpg"

---

ASP.NET Core has in-built dependency injection container and it's pretty good enough to use. 
I use TestApiFactory class to use it without too much set up, but this time, I had to wire up service provider myself, as thses tests run against Service Fabric worker process which is an executable. 

By the way, what I mean by "Component Testing" is you [test your code against public interface](https://www.youtube.com/watch?v=EZ05e7EMOLM). The benefit is you couple your tests not to implementation of your code but to public interface, and as a result, tests tend to stay unchanged even though you are chaning your implementation of feature massively. 
I only mock the external dependencies such as repositories and message publishers (Azure ServiceBus / AWS SNS)

Wiring up service container / provider is surprisingly simple to use. 

```csharp
[TestFixture]
public class Tests
{
    private Mock<IStoreEvent> _eventStore
    private ServiceProvider _serviceProvider
    
    public Tests() {
        _eventStore = new Mock<IStoreEvent>();
        _serviceProvider = new ServiceCollection()
            .AddMediatR(typeof(Worker).Assembly)
            .AddTransient(s => _eventStore.object)
            .BuildServiceProvider();
    }
    
    [Test]
    public async Task Should_retrieve_message() {
        // arrange
        var mediator = _serviceProvider.GetService<IMediator>();
        var processor = new Processor(mediator);
        
        // act
        await processor.Run();
        
        // assert
        _eventStore.Verify(s => s.List());
    }
}
```


# Weapsy.Mediator

[![Build status](https://ci.appveyor.com/api/projects/status/p5p80y0fa6e9wbaa?svg=true)](https://ci.appveyor.com/project/lucabriguglia/weapsy-mediator)

Mediator for .NET Core

## Installing Weapsy.Mediator

[Nuget Package](https://www.nuget.org/packages/Weapsy.Mediator/)

Via Package Manager

    Install-Package Weapsy.Mediator
    
Or via .NET CLI

    dotnet add package Weapsy.Mediator
    
Or via Paket CLI

    paket add Weapsy.Mediator

## Using Weapsy.Mediator


### Register services

In this example I'm using Scrutor but you can use the IOC of your choice.

```C#
services.Scan(s => s
    .FromAssembliesOf(typeof(IMediator), typeof(DoSomething))
    .AddClasses()
    .AsImplementedInterfaces());
```
DoSomething is an sample command and the assumption is that all the handlers are in the same assembly.

### Basics

Note that all handlers are available as asynchronous and as well as synchronous, but for these examples I'm using the asynchronous ones only.

There are 3 kinds of messages:
- Command, single handler
- Event, multiple handlers
- Query/Result, single handler that returns a result

#### Command

First, create a message:

```C#
public class DoSomething : ICommand
{
}
```

Next, create the handler:

```C#
public class DoSomethingHandlerAsync : ICommandHandlerAsync<DoSomething>
{
    public Task HandleAsync(DoSomething command)
    {
        await _myService.MyMethodAsync();
    }
}
```

And finally, send the command using the mediator:

```C#
var command = new DoSomething();
await _mediator.SendAsync(command)
```

#### Event

First, create a message:

```C#
public class SomethingHappened : IEvent
{
}
```

Next, create one or more handlers:

```C#
public class SomethingHappenedHandlerAsyncOne : IEventHandlerAsync<SomethingHappened>
{
    public Task HandleAsync(SomethingHappened @event)
    {
        await _myService.MyMethodAsync();
    }
}

public class SomethingHappenedHandlerAsyncTwo : IEventHandlerAsync<SomethingHappened>
{
    public Task HandleAsync(SomethingHappened @event)
    {
        await _myService.MyMethodAsync();
    }
}
```

And finally, send the event using the mediator

```C#
var @event = new SomethingHappened();
await _mediator.PublishAsync(@event)
```

..._work in progress_...

















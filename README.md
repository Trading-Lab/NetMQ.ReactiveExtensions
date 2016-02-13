# NetMQ.ReactiveExtensions

[![NuGet](https://img.shields.io/nuget/v/NetMQ.ReactiveExtensions.svg)](https://www.nuget.org/packages/NetMQ.ReactiveExtensions/) ![](https://img.shields.io/appveyor/ci/drewnoakes/netmq-reactiveextensions.svg)

Effortlessly send messages anywhere on the network using Reactive Extensions (RX). Uses NetMQ as the transport layer.

## Sample Code

The API is a drop-in replacement for `Subject<T>` from Reactive Extensions (RX).

As a refresher, to use `Subject<T>` in Reactive Extensions (RX):

```csharp
var subject = new Subject<int>();
subject.Subscribe(message =>
{
	// Receives 42.
});
subject.OnNext(42);
```

The new API is virtually identical:

```csharp
var subject = new SubjectNetMQ<int>("tcp://127.0.0.1:56001");
subject.Subscribe(message =>
{
	// Receives 42.
});
subject.OnNext(42);
```

If we want to run in separate processes:

```csharp
// Machine 1
var subject = new SubjectNetMQ<int>("tcp://127.0.0.1:56001");
subject.Subscribe(message =>
{
	// Receives 42.
});

// Machine 2
var subject = new SubjectNetMQ<int>("tcp://127.0.0.1:56001");
subject.OnNext(42); // Sends 42.
```

Currently, serialization is performed using [ProtoBuf](https://github.com/mgravell/protobuf-net "ProtoBuf"). It will handle simple types such as `int` without annotation, but if we want to send more complex classes, we have to annotate like this:

```csharp
[ProtoContract]
public struct MyMessage
{
	[ProtoMember(1)]
	public int Num { get; set; }
	[ProtoMember(2)]
	public string Name { get; set; }
}
```

## NuGet Package

![](https://img.shields.io/nuget/v/NetMQ.ReactiveExtensions.svg)

See [NetMQ.ReactiveExtensions](https://www.nuget.org/packages/NetMQ.ReactiveExtensions/).

## Demos

To check out the demos, see:
- Publisher: Project `NetMQ.ReactiveExtensions.SamplePublisher`
- Subscriber: Project `NetMQ.ReactiveExtensions.SampleSubscriber`
- Sample unit tests: Project `NetMQ.ReactiveExtensions.Tests`

## Notes

- Compatible with all existing RX code. Can use `.Where()`, `.Select()`, `.Buffer()`, `.Throttle()`, etc.
- Runs at >120,000 messages per second on localhost.
- Supports `.OnNext()`, `.OnException()`, and `.OnCompleted()`.
- Properly passes exceptions across the wire.
- Supported by a full suite of unit tests.

## Wiki

See the [Wiki with more documentation](https://github.com/NetMQ/NetMQ.ReactiveExtensions/wiki).


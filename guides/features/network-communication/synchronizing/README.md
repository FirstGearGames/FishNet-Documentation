# SyncTypes

Like[ remote procedure calls](../remote-procedure-calls.md), **SyncTypes** are another type of [communication](../../../high-level-overview/terminology/communicating.md). These are fields which automatically synchronize over the network to clients when the server changes them. There are a variety of **SyncTypes** available: [SyncVar](syncvar.md), [SyncDictionary](syncdictionary.md), [SyncList](synclist.md), [custom SyncTypes](custom-synctype.md), and more.

When changes are made to a SyncType, only the changes are sent. For example, if you have a SyncList of 10 values and add another, only the just added entry will be sent.

Any changes made to SyncTypes in Awake will be performed on server and client without requiring synchronization. This is a great opportunity to add to lists or dictionaries. If you wish values to be synchronized after initializing use OnStartServer.

{% hint style="warning" %}
SyncVar Values will be reset before `OnDestroy` runs on the object, so you won't be able to get them in that method.
{% endhint %}

By altering the **SendRate,** you can slow down how often SyncTypes can at most often sync their changes. This can be useful if you make several changes in quick succession and only want the latest change to be sent every couple of seconds for example. The values are per second, and setting a SendRate of `0f` will allow SyncTypes to send changes as often as possible, which is every network tick if your game is running on a [non-variable timing mode](../../../../fishnet-building-blocks/components/managers/time-manager.md#timing-type).&#x20;

SyncTypes will also [automatically generate serializers](../../data-serialization/) for custom types.

{% hint style="info" %}
When SyncTypes are set to use reliable messaging (the default), their changes will arrive on clients in the reverse order that they are defined inside the script. This is due to the way the code generation works.
{% endhint %}

## Host client limitations

There is a small limitation with all **SyncTypes** when running both the client and server in a single build.

While as the host client, the previous value, when applicable, in callbacks will be the current value or null if the `asServer` parameter is false. This is mostly noticed in [SyncVars](syncvar.md), note the example below:

```csharp
// This example assumes you are acting as the host client.
// We will imagine previous value was 10, and next is 20.
private void _mySyncVar_OnChange(int prev, int next, bool asServer)
{
    // If asServer if true then the prev value will correctly be 10,
    // with the next being 20.
    
    // If asServer is false then the prev value will be 20, as well the
    // next being 20. This is because the value has already been changed
    // on the server, and the previous value is no longer stored.
    
    DoSomething(prev, next);
}
```

There is a fairly simple way to accommodate this scenario if you plan on using a host client in your game.

```csharp
// This example assumes you are acting as the host client.
// We will imagine previous value was 10, and next is 20.
private void _mySyncVar_OnChange(int prev, int next, bool asServer)
{
    // Only run the logic using the previous value if being called
    // with asServer true, or if only the client is started.
    if (asServer || IsClientOnlyStarted)
        DoSomething(prev, next);
}
```

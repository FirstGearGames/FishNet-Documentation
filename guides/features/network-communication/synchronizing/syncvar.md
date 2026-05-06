---
description: >-
  SyncVars are the most simple way to automatically synchronize a single
  variable over the network.
---

# SyncVar

SyncVars are used to synchronize a single field. Your field can be virtually anything: a value type, struct, or class. To utilize a SyncVar you must implement your type as a SyncVar class

```csharp
public class YourClass : NetworkBehaviour
{
    private readonly SyncVar<float> _health = new SyncVar<float>();
}
/* Any time _health is changed on the server the
* new value will be sent to clients. */
```

SyncTypes can also be customized with additional options by using the UpdateSettings method within your declared SyncTypes, or using the initializer of your SyncType. These options include being notified when the value changes, changing how often the SyncType will synchronize, and more. You can view a full list of SyncVar properties which may be changed by viewing the [API](https://fish-networking.com/FishNet/api/api/FishNet.Object.Synchronizing.SyncVarAttribute.html#fields).

Below is a demonstration on sending SyncTypes at a longer interval of at most every 1f, and being notified of when the value changes.

```csharp
private readonly SyncVar<float> _health = new SyncVar<float>(new SyncTypeSettings(1f));
    
private void Awake()
{
    _health.OnChange += on_health;
}

// This is called when _health changes, for server and clients.
private void on_health(float prev, float next, bool asServer)
{
    /* Each callback for SyncVars must contain a parameter
    * for the previous value, the next value, and asServer.
    * The previous value will contain the value before the
    * change, while next contains the value after the change.
    * By the time the callback occurs the next value had
    * already been set to the field, eg: _health.
    * asServer indicates if the callback is occurring on the
    * server or on the client. Sometimes you may want to run
    * logic only on the server, or client. The asServer
    * allows you to make this distinction. */
}
```

Another common request is achieving client-side SyncVar values. This may be achieved by using a ServerRpc.

{% hint style="warning" %}
When modifying SyncVars using client-side, a RPC is sent with every set. If your SyncVar value will change frequently consider limiting how often you set the value client-side to reduce bandwidth usage.

In a future release we are planning to make all SyncTypes have client-authoritative properties where this work-around will not be needed.
{% endhint %}

```csharp
// A typical server-side SyncVar.
public readonly SyncVar<string> Name = new SyncVar<string>();
// Create a ServerRpc to allow owner to update the value on the server.
[ServerRpc] private void SetName(string value) => Name.Value = value;
```

When using client-side SyncVars you may want to consider ExcludeOwner in the SyncVar ReadPermissions to prevent owners from receiving their own updates. In addition, using WritePermission.ClientUnsynchronized will allow the client to set the value locally. Lastly, RunLocally as true in the ServerRpc will execute the RPC code on both the sender(client) and the server.

Using ExcludeOwner as the SyncVar ReadPermissions, and ClientUnsynchronized in the WritePermissions. . In the example below we also set RunLocally to true for the ServerRpc so that the calling client also sets the value locally.

```csharp
// Attributes shown in previous examples can stack but they were removed
// here for simplicity.
private readonly SyncVar<string> Name = new SyncVar<string>(new SyncTypeSettings(WritePermission.ClientUnsynchronized, ReadPermission.ExcludeOwner));
// Create a ServerRpc to allow owner to update the value on the server.
[ServerRpc(RunLocally = true)] private void SetName(string value) => Name.Value = value;
```

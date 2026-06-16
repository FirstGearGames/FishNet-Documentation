---
description: >-
  Information about how to run methods across the network using remote procedure
  calls.
---

# Remote Procedure Calls

Remote Procedure Calls (RPCs) are a type of [communication](../../high-level-overview/terminology/communicating.md#server-and-host-2) that are received on the same object they are sent from. The three types of RPCs are ServerRpc, ObserversRpc, and TargetRpc. RPCs are easy to use and are just like calling another method. Since Remote Procedure Calls are object bound, they must be called on scripts which inherit from [NetworkBehaviour](../../../fishnet-building-blocks/components/network-behaviour-components/).

Fish-Net will [generate serializers](../data-serialization/) for your types automatically, including arrays and lists. In rare cases Fish-Net may not be able to create a serializer for you. When this occurs you must create a [custom serializer](../data-serialization/custom-serializers-guides/).

{% hint style="info" %}
Remote Procedure Call methods do not need to have prefixes as shown in the examples, but it's good practice to use them to keep your intentions known.
{% endhint %}

***

## ServerRpc <a href="#serverrpc" id="serverrpc"></a>

A ServerRpc allows a client to run logic on the server. The client calls a ServerRpc method, and the data to execute that method is sent to the server. To use a ServerRpc the client must be active, and the method must have a _ServerRpc_ attribute.

```csharp
private void Update()
{
    // If owner and space bar is pressed.
    if (base.IsOwner && Input.GetKeyDown(KeyCode.Space))
        RpcSendChat("Hello world, from owner!");        
}

[ServerRpc]
private void RpcSendChat(string msg)
{
    Debug.Log($"Received {msg} on the server.");
}
```

By default only the [owner](../ownership/) of an object may send a ServerRpc. It is however possible to allow ServerRpcs to be called by any client, owner or not. This is done by setting RequireOwnership to false in the _ServerRpc_ attribute. You may optionally know which connection is calling the RPC by using a NetworkConnection type with a null default as the last parameter. The NetworkConnection parameter will automatically be populated with whichever client is calling the ServerRpc; you do not need to specify the connection.

```csharp
[ServerRpc(RequireOwnership = false)]
private void RpcSendChat(string msg, NetworkConnection conn = null)
{
    Debug.Log($"Received {msg} on the server from connection {conn.ClientId}.");
}
```

***

## ObserversRpc <a href="#observersrpc" id="observersrpc"></a>

ObserversRpc allows the server to run logic on clients. Only observing clients will get and run the logic. Observers are set by using the [Network Observer](../../../fishnet-building-blocks/components/network-observer.md) component. To use ObserversRpc add the _ObserversRpc_ attribute to a method.

```csharp
private void FixedUpdate()
{
    RpcSetNumber(Time.frameCount);
}

[ObserversRpc]
private void RpcSetNumber(int next)
{
    Debug.Log($"Received number {next} from the server.");
}
```

In some instances you might want the owner of an object to ignore received ObserversRpc. To accomplish this set ExcludeOwner to true on the _ObserversRpc_ attribute.

```csharp
[ObserversRpc(ExcludeOwner = true)]
private void RpcSetNumber(int next)
{
    // This won't run on owner.
    Debug.Log($"Received number {next} from the server.");
}
```

You may also have the latest values sent through an ObserversRpc also automatically send to new clients. This can be useful for values that only change through RPCs. Doing so is done by setting BufferLast to true on the _ObserversRpc_ attribute. The example below will not send to owner, and will also update new joining clients.

```csharp
[ObserversRpc(ExcludeOwner = true, BufferLast = true)]
private void RpcSetNumber(int next)
{
    // This won't run on owner and will send to new clients.
    Debug.Log($"Received number {next} from the server.");
}
```

***

## TargetRpc <a href="#targetrpc" id="targetrpc"></a>

Lastly is TargetRpc. This RPC is used to run logic on a specific client. You can implement this feature by adding the _TargetRpc_ attribute. When sending a TargetRpc the first parameter of your method must always be a NetworkConnection; this is the connection the data goes to.

```csharp
private void UpdateOwnersAmmo()
{
    /* Even though this example passes in owner, you can send to
    * any connection that is an observer. */
    RpcSetAmmo(base.Owner, 10);
}

[TargetRpc]
private void RpcSetAmmo(NetworkConnection conn, int newAmmo)
{
    // This might be something you only want the owner to be aware of.
    _myAmmo = newAmmo;
}
```

***

## Multi-Purpose Rpc

It is possible to have a single method be both a TargetRpc, as well an ObserversRpc. This can be very useful if you sometimes want to send a RPC to all observers, or a single individual. A chat message could be an example of this.

```csharp
[ObserversRpc][TargetRpc]
private void DisplayChat(NetworkConnection target, string sender, string message)
{
    Debug.Log($"{sender}: {message}."); // Display a message from sender.
}

[Server]
private void SendChatMessage()
{
    // Send to only the owner.
    DisplayChat(base.Owner, "Bob", "Hello world");
    // Send to everyone.
    DisplayChat(null, "Bob", "Hello world");
}
```

***

## Channels <a href="#channels" id="channels"></a>

Remote procedure calls can be sent as **reliable or unreliable**. To utilize this feature add a _Channel_ type as the last parameter to your RPC. You may also use your added Channel parameter to know which Channel the data arrived on.

```csharp
private bool _reliable;
private void FixedUpdate()
{
    // Reliable or unreliable can be switched at runtime!
    _reliable = !_reliable;
    
    Channel channel = (_reliable) ? Channel.Reliable : Channel.Unreliable;
    RpcTest("Anything", channel);
}

/* This example uses ServerRpc, but any RPC will work.
* Although this example shows a default value for the channel,
* you do not need to include it. */
[ServerRpc]
private void RpcTest(string txt, Channel channel = Channel.Reliable)
{
    if (channel == Channel.Reliable)
        Debug.Log("Message received! I never doubted you.");
    else if (channel == Channel.Unreliable)
        Debug.Log($"Glad you got here, I wasn't sure you'd make it.");
}
```

If your RPC is a ServerRpc and you are using a NetworkConnection with a null default as the last parameter then the Channel parameter must be added second to last, just before the NetworkConnection parameter.

```csharp
/* Example of using Channel with a ServerRpc that
* also provides the calling connection. */
[ServerRpc(RequireOwnership = false)]
private void RpcTest(string txt, Channel channel = Channel.Reliable, NetworkConnection sender = null)
{
    Debug.Log($"Received on channel {channel} from {sender.ClientId}.");
}
```

***

## RunLocally

All RPC types may use the RunLocally field within the RPC attribute. When RunLocally is true the method will send, but also run on the calling side. For example, if a client calls a ServerRpc with RunLocally true, the ServerRpc will be put in the clients buffer to send, then the client will run the method logic locally.

```csharp
// Keeping in mind all RPC types can use RunLocally.
[ServerRpc(RunLocally = true)]
private void RpcTest()
{
    // This debug will print on the server and the client calling the RPC.
    Debug.Log("Rpc Test!");
}
```

{% hint style="info" %}
Only transports which support unreliable messages may send RPCs as unreliable. If you attempt to send a RPC as unreliable when the transport does not support it the RPC will default to reliable.
{% endhint %}

***

## DataLength

If you have intentions to send very large amounts of data you may specify the maximum potential size of your data in the RPC attribute. Doing so will use a reserved serializer to prevent resizing and allocations, increasing performance. Every RPC type supports this feature.

```csharp
// This implies we know the data will never be larger than 3500 bytes.
// If the data is larger then a resize will occur resulting in garbage collection.
[ServerRpc(DataLength = 3500)]
private void ServerSendBytes(byte[] data) {}
```

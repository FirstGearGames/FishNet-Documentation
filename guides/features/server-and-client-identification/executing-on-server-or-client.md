---
description: >-
  How you can run certain code on only the server, or only a client, or only on
  a host.
---

# Executing on Server or Client

## Overview

Very often your code will need to know if it's running on a server or a client, or [both at the same time](#user-content-fn-1)[^1]. There are several ways of knowing this, for example, code executed directly from client or server only events or methods, checking compiler directives to see the type of game build, marking methods to only allow running on a specific instance, or simply checking a Boolean value.

***

## Checking a Boolean

{% tabs %}
{% tab title="Description" %}
You can easily check if the current code is running on the server with a Boolean check. FishNet's [NetworkBehaviour](../networked-gameobjects-and-scripts/network-behaviour-guides.md) and [NetworkObject](../networked-gameobjects-and-scripts/networkobjects/) classes have easy to use properties for this exact reason. They are also accessible from the [NetworkManager](../../../fishnet-building-blocks/components/managers/network-manager.md) directly.

You can use `IsClientStarted` to detect if the instance running the code is a client (it could also be a server).

`IsClientOnlyStarted` can be used to detect if the instance running the code is a client only and not a server.

`IsServerStarted` detects if the instance running the code is the server (It could also be a client).

And finally, `IsServerOnlyStarted` returns whether the instance running the code is a server only and not a client.

When checking from a **NetworkBehaviour** or on a **NetworkObject** you will often want to know if the code is running on the client or server side AND the **NetworkObject** has already been initialized on the network and can be safely used for network functionality, such as RPCs or network properties. You can do that using the `IsServerInitialized`, `IsServerOnlyInitialized`, `IsClientInitialized`, and `IsClientOnlyInitialized` properties.

{% hint style="success" %}
Instead of using something like `IsClientInitialized && IsServerInitialized` each time you want to check if the code is running on the host, you can simply check `IsHostInitialized`. There are _Started_, and _Only_ variations of this as well.
{% endhint %}
{% endtab %}

{% tab title="Example" %}
Simple example showing how a NetworkBehaviour could prevent code from running on the server:

{% code title="Player.cs" %}
```csharp
using FishNet.Object;

public class Player : NetworkBehaviour
{
    public void OnStruck()
    {
        if (!IsClientInitialized)
            return;

        // Play visual effect and sounds only on a client.
    }
}

```
{% endcode %}
{% endtab %}
{% endtabs %}

***

## Method attributes

There are a variety of attributes which can be used to ensure a method only runs on the appropriate game instance. Unlike properties, attributes are NetworkBehaviour-independent and can be used in any type of class or struct.

{% hint style="success" %}
These methods are also used for Fish-Networking Pro's automatic code stripping. Importantly, methods marked with the `Server` attribute will have their method body stripped from non-server builds.
{% endhint %}

### Client attribute

{% tabs %}
{% tab title="Description" %}
Placing a `Client` attribute above a method ensures that the method cannot be called unless the local client is active and the **NetworkObject** is initialized (the latter of which can be disabled by enabling the `UseIsStarted` option). There are additional properties which may be added to the attribute for additional functionality, such as for disabling the warning that would be logged if you try to call the method with this and it doesn't pass the check.
{% endtab %}

{% tab title="Example" %}
Simple example showing the `Client` attribute to prevent the method from running when not a client.

```csharp
[Client]
private void ShowUI()
{
    // This code will only run on a client, otherwise it will print a warning.
}
```

And following is an example of the options you can use with it:\
If this method were called with no client active a warning would be printed and the method would not run. You can change the logging type using the `Logging` property to change whether it should log a warning at all or not. The default value is `LoggingType.Warning`.

It's also possible to allow only the owner of the object to call the method by setting `RequireOwnership` to true; this value is false by default.

Finally, you can use the `UseIsStarted` property to cause this to ignore the initialized state of the **NetworkObject** and check if the client alone is started.

```csharp
// The server does not need to play VFX; only play VFX if the client is active.
[Client(Logging = LoggingType.Off, RequireOwnership = true, UseIsStarted = true)]
private void PlayVFX() 
{ 
    // Play VFX here...
}
```
{% endtab %}
{% endtabs %}

### Server attribute

{% tabs %}
{% tab title="Description" %}
The `Server` attribute provides similar features of the `Client` variant, except it will not allow the method to run if the **server** is not active and the **NetworkObject** is initialized (the latter of which can be disabled by enabling the `UseIsStarted` option). There are additional properties which may be added to the attribute for additional functionality, such as for disabling the warning that would be logged if you try to call the method with this and it doesn't pass the check.
{% endtab %}

{% tab title="Example" %}
Here is an example of the `Server` attribute being used to prevent code from running anywhere except the server.

Like the Client attribute, a warning will be thrown if this is called while the server is not active.\
The warning can be changed or disabled by adjusting the `Logging` property.

You can use the `UseIsStarted` property to cause this to ignore the initialized state of the **NetworkObject** and only check if the server is started.

```csharp
// The server would validate hit results from a client.
[Server(Logging = LoggingType.Off, UseIsStarted = false)]
private void ValidateHit() 
{
    // Hit validation code here that will only run on the server.
}
```
{% endtab %}
{% endtabs %}

[^1]: This is what we refer to as a host.

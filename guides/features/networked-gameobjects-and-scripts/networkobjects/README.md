---
description: >-
  Details on the different types of NetworkObjects that will be referenced
  throughout the guides.
---

# NetworkObjects

## NetworkObject

Any GameObject with a [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) component on it will be considered a "NetworkObject" in these guides going forward.\
\
Review the [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) component Page for details on the various settings for a **NetworkObject.**

When you are to add a [**NetworkBehaviour**](../../../../fishnet-building-blocks/components/network-behaviour-components.md) component to your prefabs or scene objects the NetworkBehaviour will search for a NetworkObject component on the same object, or within parent objects. If a NetworkObject is not found then one will be added automatically to the top-most object.

## Spawned NetworkObject

NetworkObjects that are Instantiated and Spawned using ther ServerManager.Spawn() method will be considered a "Spawned NetworkObject".\
\
"IsSpawned" Property will be marked True internally on the NetworkObject Component.

## Scene NetworkObject

Any **NetworkObject** that exists as part of the Scene aka - never instantiated/spawned into the scene, will be considered a "**Scene NetworkObject**" in these guides going forward.\
\
"IsSceneObject" property will be marked true on the attached [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) component internally.

## Global NetworkObject

Any **NetworkObject** with their bool "IsGlobal" marked "true" either in the inspector or with code will be considered a "**GlobalNetworkObject**" in the guides going forward.

**GlobalNetworkObjects** will automatically be put into the ("DontDestroyOnLoad") scene when instantiated on the server, and when spawned on the clients.

{% hint style="info" %}
Scene objects cannot be marked as global. All global objects must be instantiated and spawned.
{% endhint %}

## Nested NetworkObject

Any [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) that are a child of another [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) will considered a "**Nested NetworkObject**" in these guides going forward.\
\
"IsNested" property will be marked true on the attached [**NetworkObject**](../../../../fishnet-building-blocks/components/network-object.md) component internally.

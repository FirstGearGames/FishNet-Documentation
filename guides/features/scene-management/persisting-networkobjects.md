---
description: >-
  This page will go over details on the options available to users for
  persisting NetworkObjects across scene loading and unloading
---

# Persisting NetworkObjects

## General

The options available to users to keep [**NetworkObjects**](../networked-gameobjects-and-scripts/networkobjects/) persisting across scenes depends on the type of NetworkObject.

## Spawned NetworkObjects

[**Spawned NetworkObjects**](../networked-gameobjects-and-scripts/networkobjects/#spawnednetworkobject) that do not fall into the other categories below can persist between scenes by moving them while loading into the next scene.\
\
The [**SceneLoadData**](scene-data/sceneloaddata.md) that is passed into the Load Method of the SceneManager has an array that you can populate with all of the Spawned NetworkObjects you want to send to the new loaded scene.

The SceneManager will handle the objects correctly for you and also flag a Debug if you try to send a GameObject that is not allowed.

{% hint style="warning" %}
If you load Multiple Scenes in one method call, and you are moving network objects using SceneLoadData. The moved networkobjects will move into the first valid scene requested.
{% endhint %}

#### Example

```csharp
// Just create an array of NetworkObjects 
// that are not a Scene, Global or Nested NetworkObjects.
NetworkObject[] objectsToMove = new NetworkObject[] { object1, object2, object3 }

// Assign this array to the SceneLoadData before you Load a Scene.
SceneLoadData sld = new SceneLoadData("NewScene");
sld.MovedNetworkObjects = objectsToMove;

// FishNet will handle the rest after loading!
SceneManager.LoadGlobalScenes(sld);
```

## Scene NetworkObjects

[**Scene NetworkObjects**](../networked-gameobjects-and-scripts/networkobjects/#scenenetworkobject) currently cannot persist across scenes, it is a [**limitation**](../../troubleshooting/technical-limitations.md) with the way Unity and FishNet was designed. You cannot mark them as IsGlobal, or put them into the "DontDestroyOnLoad" scene. If you would like a Scene NetworkObject to persist across scenes it is recommended to remove them and use the other options available on this page.

## Global NetworkObjects

<mark style="color:blue;">**G**</mark>[<mark style="color:blue;">**lobal NetworkObjects**</mark>](../networked-gameobjects-and-scripts/networkobjects/#globalnetworkobject) work similar to how a normal GameObject would when put into the "DontDestroyOnLoad" (DDOL) scene.\
\
When loading and unloading scenes, **Global NetworkObjects** will stay in the (DDOL) scene on both the server and client persisting their state. No extra steps needed.

To make a NetworkObject global just mark the IsGlobal Boolean "true" on the NetworkObject component.

{% hint style="info" %}
Clients typically are always an observer of the DDOL scene, so global objects may make more sense for Manager type GameObjects, instead of gameobjects that have meshes, but you are not limited to this.
{% endhint %}

## Nested NetworkObjects

Unity will not allow [**Nested GameObjects**](../networked-gameobjects-and-scripts/networkobjects/nested-networkobjects.md) to be moved into other scenes.\
\
However! Fishnet will automatically detect if you are trying to send a NestedNetworkObject and send the root of the object instead!

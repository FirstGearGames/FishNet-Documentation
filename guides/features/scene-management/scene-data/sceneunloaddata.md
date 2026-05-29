---
description: >-
  The Data Class needed for the SceneManager to know how to handle unloading a
  scene.
---

# SceneUnloadData

## General

When unloading scenes information on what to unload is constructed within the SceneUnloadData class. SceneUnloadData is very similar to SceneLoadData. The API for SceneUnloadData can be found [here](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Scened.SceneUnloadData.html).

## Default values

```csharp
SceneUnloadData sud = new SceneUnloadData()
{
    SceneLookupDatas = new SceneLookupData[0],
    Params = new UnloadParams()
    {
        ServerParams = new object[0],
        ClientParams = new byte[0]
    },
    Options = new UnloadOptions()
    {
        Mode = ServerUnloadMode.UnloadUnused,
        Addressables = false
    }
};
```

<details>

<summary>SceneLookupDatas</summary>

This Array is populated with the scenes you want to unload, depending on the parameters you pass into the SceneUnloadData when constructed.

See [**Unloading Scenes**](../unloading-scenes.md) for examples.

</details>

<details>

<summary>Params</summary>

Params are an optional way to assign data to your scene loads/unloads. This data will be available within[ **Scene Events**](../scene-events.md), Information used in Params can be useful for storing information about the scene load/unload and referencing it later when the scene load/unload completes.

**ServerParams**

_ServerParams_ are only included on the server side and are not networked. It is an array of objects, meaning you can send anything you want. However when accessing the Params through event args, you will have to cast the object to the data you want.

**ClientParams**

_ClientParams_ is a byte array which may contain anything. It will be sent to clients when they receive the load scene instructions. Clients can access the _ClientParams_ within the scene change events.

</details>

<details>

<summary>Options</summary>

Like with Options in loading, the UnloadOptions offer additional settings when unloading.

**Mode**

These values will override the AutomaticallyUnload Option that was used when loaded the scene. If you set _AutomaticallyUnload_ to false but specified _ServerUnloadModes.UnloadUnused_ then the scene would be unloaded when emptied.

**ServerUnloadModes.UnloadUnused**

* This is the default setting which will only unload a scene which is no longer used.

**ServerUnloadModes.KeepUnused**

* This option will keep the scene loaded on the server if all clients have been removed. See [**Scene Caching**](../scene-caching.md) for more details

</details>

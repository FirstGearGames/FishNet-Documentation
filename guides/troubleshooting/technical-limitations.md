# Technical Limitations

Limitation is not a word we like to use in Fish-Networking, but unfortunately there are a few technical blocks with Unity, or restrictions in place for performance reasons. These limitations are considered reasonably acceptable and may never be resolved.

<details>

<summary>Networked scene objects</summary>

When a scene object is networked they may behave differently.

A networked scene object will be disabled when a scene loads, and will not activate until the client or server is started. For clients specifically, networked scene objects will only activate when the server is sure the client has loaded the scene; this is done automatically through the Fish-Networking scene manager.

Our [SceneManager](../../fishnet-building-blocks/components/managers/scenemanager.md) allows instantiated networked objects to be moved between scenes, but networked scene objects may not. Unity cannot know a scene objects details without the scene being loaded first, so trying to spawn a scene object without the client having the scene loaded would result in errors.

Due to the movement restrictions just mentioned, networked scene objects may not be marked **DontDestroyOnLoad**, nor can the [NetworkObject.IsGlobal](../features/networked-gameobjects-and-scripts/networkobjects/#global-networkobject) feature be used. Both of these would place the scene object in a new scene, causing errors.

When a networked scene object is despawned it is always disabled, rather than destroyed. This is so you may spawn it at a later time. Manually destroying a scene object on the server is possible and would simply result in it never being spawned on clients.

</details>

<details>

<summary>Nested NetworkObjects and NetworkBehaviours</summary>

NetworkObjects which are nested on a prefab may not be unparented at runtime.

Root NetworkObjects may have their parent updated at runtime.

</details>

<details>

<summary>PredictedSpawn</summary>

Predicted spawns currently cannot be spawned as nested.

Predicted spawns are currently not aligned with prediction interpolation.

</details>

<details>

<summary>SyncType host client callbacks</summary>

There is a small limitation with all **SyncTypes** when running both the client and server in a single build.

While as the host client, the previous value, when applicable, in callbacks will be the current value or null if the `asServer` parameter is false. There is a workaround for this here: [#host-client-limitations](../features/network-communication/synchronizing/#host-client-limitations "mention")

</details>

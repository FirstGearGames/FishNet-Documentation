---
description: Discover frequently asked questions and their answers.
---

# FAQ

## Miscellaneous

<details>

<summary>How do I send some initial data to the server as soon as I connect and before my player spawns?</summary>

Broadcast are generally the best choice for sending data without a player object. Many developers trade this data in a custom [authenticator ](../../fishnet-building-blocks/components/utilities/authenticator.md)class, which allows data to be sent before the client initializes anything at all for the network. See our PasswordAuthenticator script for an example of doing this.

Another approach is to use a custom player spawner instead of our [PlayerSpawner](../../fishnet-building-blocks/components/playerspawner.md). You may send broadcasts back and forward freely, and only spawn your player when you feel is right. See [broadcasts](../features/network-communication/broadcasts.md) for more information on this feature.

</details>

<details>

<summary>Why can I see my player object but not control it?</summary>

Most often when this occurs you actually are able to control your player, but you simply do not see them because your game could be using the incorrect camera. This is seen when more than one player spawns, and the player prefab has a live camera beneath them.

To resolve this issue simply use one camera in the scene and take over it as needed, or disable the camera on your player prefab and only enable it if you own the object.

</details>

<details>

<summary>How do I update to a new or Pro version of Fish-Networking?</summary>

You can install Fish-Networking free over Pro, and Pro over free without any issues. Just download free or Pro and import normally. See our [Pro section](../../overview/readme/pro-projects-and-support.md) for information on downloading Pro.

If you import a new version of Fish-Networking and there are immediately compile errors, delete your FishNet folder then import the latest version again.

</details>

## Hosting and connectivity

<details>

<summary>How do I make rooms in Fish-Networking?</summary>

There are several ways to make rooms within Fish-Networking.

You can use a third party service which creates individual server instances, each acting as their own. There are several services which provide this, an example of one is [PlayFlow Cloud](../server-hosting/services/playflow-cloud/).

Another option is to have a single Fish-Networking instance manage rooms in a single build. This reduces the complexity of a third party service but requires you to develop with [stacked scenes](../features/scene-management/scene-stacking.md) in mind. Our project [Lobby and Worlds ](../../overview/readme/pro-projects-and-support.md#projects)accomplishes this, and is available to supporters.

While stacked scenes aren't necessarily difficult, and support for them are built-into Fish-Networking, there are still some Unity limitations around them. An example being, not all physics API are available in stacked scenes. See [Physics Scenes](https://docs.unity3d.com/ScriptReference/PhysicsScene.html) for more information; there is also PhysicsScene2D.

</details>

<details>

<summary>Why are my Meta Quest/mobile players not immediately disconnecting when they close the game?</summary>

Mobile apps don't typically close like a normal application. Instead, mobile apps are suspended so that you may "re-open" them quickly.

Clients will likely disconnect if the game remains in the background for longer than the timeout settings on your Client/ServerManager.

Often you can utilize the Unity callback OnApplicationPause in mobile games to tell the ClientManager to disconnect.

</details>

<details>

<summary>How can I have p2p (player hosted) games without paying for dedicated servers?</summary>

A large variety of third party services allow you to host p2p games. Steam and EOS are the two most common ones, but plenty are available. We support both Steam and EOS.

</details>

<details>

<summary>Why don't my games connect when I press server on one device and client on another even on a LAN?</summary>

When trying to connect to your IP directly you must allow connections through your firewall. Be sure to adjust your firewall to allow the port used by your game.

To help you find and join LAN games, you can use one of the Network Discovery addons [Fish-Networking Discovery](https://github.com/abdelfattahradwan/Fish-Networking-Discovery) or [Unity Network Discovery](https://github.com/FrantisekHolubec/UnityNetworkDiscovery).

</details>

<details>

<summary>What can I do if I encounter a "port already used"?</summary>

Check to make sure you aren't trying to start the FishNet server twice, perhaps due to manually starting it as well as FishNet automatically starting it if enabled in the [ServerManager ](../../fishnet-building-blocks/components/managers/server-manager.md)component's [#start-on-headless](../../fishnet-building-blocks/components/managers/server-manager.md#start-on-headless "mention") option.\
You can also try enabling the [ReuseAddress](../../fishnet-building-blocks/transports/tugboat.md#reuse-address) option if using the [Tugboat transport](../../fishnet-building-blocks/transports/tugboat.md). This will allow the server to bind to the port even if it was recently used, making server restarts and multiple concurrent instances possible without port conflicts.

</details>

## Missing or hidden objects

<details>

<summary>Why are the (mesh/sprite/etc.) renderers on my object disabled on the clientHost?</summary>

By default when the clientHost is not an observer of an object the renders for the object are disabled. This is to simulate as if the object is not spawned for the clientHost, though it of course is as the server is still using the object.

You may disable this feature by changing a setting on the [ObserverManager](../../fishnet-building-blocks/components/managers/observermanager/). It's also possible to disable this per [NetworkObject](../../fishnet-building-blocks/components/network-object.md).

You can also utilize [NetworkObject events](https://fish-networking.com/FishNet/api/api/FishNet.Object.NetworkObject.html#events) to manually update renderers.

</details>

<details>

<summary>Why are my objects in the scene disabled?</summary>

**First most, check the console for any Fish-Networking warnings or errors.**

Scene objects become disabled when the client is not an observer of the scene. Our [observer system](../features/observers/) controls what objects clients can see, spawn, and transmit.

The most common reason a client is not an observer of the scene is because the server has not added the client to the scene. This can be done manually if you know the client has the scene loaded, using a Fish-Networking SceneManager reference, and calling AddConnectionToScene. EG: sceneManager.AddConnectionToScene(yourClient).

You may just as well use our automated system but telling the SceneManager to load the scene for the client. See [this page](../features/scene-management/loading-scenes/) for more information on that.

If you are starting entering play mode with only one scene you may be manually spawning the player but not adding to starting scene. See our PlayerSpawner script within your Fish-Networking installation for an example of how to instantiate player prefabs, and add clients to scenes.

</details>

<details>

<summary>Why can my client not see objects?</summary>

Related: Why are my objects in the scene disabled?

Related: Why are the mesh/sprite/etc. renderers on my object disabled on the clientHost?

If a client is not an observer of an object then the server does not spawn the object for the client. See our [observers guide ](../features/observers/)for more information.

</details>

## SyncTypes (SyncVar, SyncList, etc.)

<details>

<summary>Why are my SyncTypes not syncing on other devices when I change it on the owner client?</summary>

Clients may update SyncTypes locally, but they are not synchronized over the network; only the server may synchronize SyncTypes. Typically, clients will send a Remote Procedure Call to the server indicating it wants to update something, and the server complies. See these guides for more information: [Remote Procedure Calls](../features/network-communication/remote-procedure-calls.md), [SyncTypes](../features/network-communication/synchronizing/).

</details>

<details>

<summary>Why do I receive remote procedure calls before SyncType updates?</summary>

SyncTimes run on intervals, defaulted to every 100ms if the SyncType has changed; the interval may be changed on the [ServerManager](../../fishnet-building-blocks/components/managers/server-manager.md).

However, even if the interval is met, SyncTypes always synchronize after remote procedure calls (RPC), even if you set them before calling the RPC. You can change SyncTypes to synchronize first on a per SyncType basis using [SyncTypeSettings](https://fish-networking.com/FishNet/api/api/FishNet.Object.Synchronizing.SyncTypeSettings.html). Review also [SyncTypes guide](../features/network-communication/synchronizing/) thoroughly for updating a SyncTypes settings.

</details>

## Remote Procedure Calls (RPCs)

<details>

<summary>How do I know who sent a ServerRpc, should I pass in the LocalConnection each time as an argument?</summary>

By default only clients which own the objects may send a ServerRpc. You may bypass this restriction by setting 'RequireOwnership' to false in the ServerRpc attribute. If you've not bypassed this restriction, the sender will always be owner.

The [ServerRpc guide ](../features/network-communication/remote-procedure-calls.md#serverrpc)shows how to set RequireOwnership, as well how to know which spectator might be sending the RPC. You will notice in the guide a NetworkConnection is specified at the end of the RPC parameters. You do not pass in a connection when sending the ServerRpc, it's set automatically when you receive the RPC call.

</details>

## Errors and warnings

<details>

<summary>Why do I get the warning: "Cannot spawn object because server is not active and predicted spawning is not enabled."?</summary>

Typically only the server may spawn networked objects. You will see this warning if you try to network spawn an object on a client, while predicted spawning is not enabled.

Predicted spawning must be turned on in the [ServerManager](../../fishnet-building-blocks/components/managers/server-manager.md). See also the [PredictedSpawn component](../../fishnet-building-blocks/components/prediction/predictedspawn.md).

</details>

<details>

<summary>Why do I get a NullReferenceException in SyncBase when an object spawns?</summary>

Most likely you are seeing this error because your SyncType does not have an initializer. When declaring SyncTypes they must be readOnly and initialized.

For example: `private readonly SyncVar<int> _mySv = new();`\
\
You can view more information about SyncTypes [here](../features/network-communication/synchronizing/).

</details>

<details>

<summary>Why do I get the warning: "Cannot complete action because client is not active."?</summary>

You will see this warning if the client is not started. It's also possible to see this warning if you are trying to communicate with the server such as using a ServerRpc before the NetworkObject has finished initializing for the client, even if the client connection has started. (`obj.IsClientInitialized`) See NetworkBehaviour [API](https://fish-networking.com/FishNet/api/api/FishNet.Object.NetworkBehaviour.html) and [callback order](../features/networked-gameobjects-and-scripts/network-behaviour-guides.md#callbacks) for more information on this.

It's also possible you have a method decorated with the '\[Client]' attribute, such as if you want a method to only run on clients. This will cause the warning, even if your intents are to not have the client connected. If this is true, you may set LoggingType to Off within the Client attribute.

</details>

<details>

<summary>Why do I get the warning: "Cannot complete action because server is not active."?</summary>

You will see this warning if the server is not started. It's also possible to see this warning if you are trying to communicate with a client such as using a Target or ObserversRpc before the object is initialized for the server. See NetworkBehaviour [API](https://fish-networking.com/FishNet/api/api/FishNet.Object.NetworkBehaviour.html) and [callback order](../features/networked-gameobjects-and-scripts/network-behaviour-guides.md#callbacks) for more information on this.

It's also possible you have a method decorated with the '\[Server]' attribute, such as if you want a method to only run on server. This will cause the warning, even if your intents are to not have the server running. If this is true, you may set LoggingType to Off within the Server attribute.

</details>

<details>

<summary>Why do I get this error? "SceneId of 4013929391 not found in SceneObjects."?</summary>

You will see this error if the server thinks your client is in scene, when the client does not have the scene loaded. Using a [SceneCondition](../../fishnet-building-blocks/scriptableobjects/observerconditions/scenecondition.md) on the [ObserverManager ](../../fishnet-building-blocks/components/managers/observermanager/)will typically resolve this problem. You can simply add the ObserverManager component to your network manager and give it the scene condition in its [Default Conditions](../../fishnet-building-blocks/components/managers/observermanager/#default-conditions) list.

If you are already using a SceneCondition and are certain the correct scenes are being loaded then open the scenes which you are having problems with, and use the [**Reserialize NetworkObjects** utility](../../fishnet-building-blocks/configuration-and-tools.md#reserialize-networkobjects) from the (**Tools → Fish-Networking** **→ Reserialize NetworkObjects**) toolbar menu.

You can also troubleshoot this further by adding the DebugManager component to your NetworkManager and enable Write Scene Object Details. The next time you see the error it will also print the scene and object name the spawn or message was intended for.

In very rare cases this is a bug. If you've tried all the troubleshooting steps above without success consider reaching us on our[ Discord](../../#external-links) for help.

</details>

<details>

<summary>Why do I get this error? "'System.Void System.ParamArrayAttribute::.ctor()' is declared in another module and needs to be imported"?</summary>

On very rare occasion you may encounter this error after making a change to your script. This is a Unity bug that we have no way to resolve internally. The exact cause is unknown, but we know it's related to Unity caching something improperly in a script.

There are a few work-arounds that have worked consistently; most commonly adding a new empty method with a parameter (try without as well) and saving the script will resolve the issue. At some point the Unity cache will fix itself, and the empty method can later be removed.

Clearing the library cache seems to have no benefits of resolving this error. The only success we've seen besides the work around is moving everything to a completely new project. Because creating a new project is so much work it's recommended to use the work-around.

</details>

## Objects

<details>

<summary>What does the message "AssetPathHash is not set for GameObject ..." mean?</summary>

There unfortunately is not any definitive cause of this error. Usually restarting Unity will resolve the problem.

This message can also be seen when a NetworkObject is a prefab which is unable to save changes. If your object is a prefab check to make sure there are no missing scripts, or anything else preventing saving of the prefab.

If issues persist please reach us on our [Discord](../../#external-links).

</details>

<details>

<summary>How do I get the player game object?</summary>

You can get a list of all objects owned by a connection by using conn.Objects.

If you want to grab the first object spawned for a connection use conn.FirstObject. You can set the FirstObject at runtime if you have a preference to the 'FirstObject'.

If you want to know what objects you own as a client you can grab your own connection under clientManager.Connection.

</details>

<details>

<summary>How can I make the player spawner spawn my player when I load the lobby into the game scene instead of immediately?</summary>

This is done by writing a custom player spawner. You can use our PlayerSpawner as an example, but instead of spawning immediately only spawn after a client has been added to a scene.

A good place to start is using the [ClientPresenceChangeEnd callback](../features/scene-management/#scene-events) to know when the client has entered the scene and has visibility of objects within it.

</details>

<details>

<summary>How do I get an object from its NetworkObject Id?</summary>

In most cases you do not need to pass around a NetworkObject Id. The recommended approach, if sending over the network, is to send the NetworkObject reference itself. This automatically efficiently sends the NetworkObject information, and returns the proper object on the other end.

Should you have reasons to use the Id specifically you can look up spawned objects within the serverManager.Objects.Spawned collection, or clientManager.Objects.Spawned if client only. Note: these collections will likely be consolidated in a later release.

</details>

## Performance

<details>

<summary>How many CCU (concurrent users) can Fish-Networking have?</summary>

Our framework does not have any CCU limitations. How many players you are able to host on a single server varies greatly depending upon your server hardware, game mechanics, and your coding efficiency.

Several Fish-Networking games have achieved 500+ CCU, and thousands of NetworkObjects.

</details>

<details>

<summary>Can you use sharding (world splitting) in Fish-Networking?</summary>

Fish-Networking does not take any special actions to support nor restrict sharding. Some users experienced success with custom implementations of world sharding, but we do not officially support this feature.

With how powerful servers have become world sharding is rarely needed, and generally we discourage against it.

</details>

---
description: To understand FishNet, it's helpful to grasp a few fundamental concepts.
cover: ../../.gitbook/assets/foundamentals-cover.png
coverY: 0
---

# Fundamentals

## A networked game

At its heart, a networked game using Fish-Networking can be thought of as a single-player game running on multiple machines (or even multiple instances of the game running on a the same machine, which is very useful for development). The magic of multiplayer comes from FishNet's ability to synchronize various parts of your game's code and state across these machines, and to send custom messages back and forth to trigger actions or update information. This ensures that all players experience a consistent and interactive world.

The code you write behaves the same as it does in a regular game, code in <mark style="color:blue;">`Update`</mark> will run each frame, <mark style="color:blue;">`Awake`</mark> triggers as soon as a script is being loaded, etc. **But** FishNet adds additional features such as [Synced Variables](../features/network-communication/synchronizing/), which you will be able to set on the game instance that is acting as the server and they will automatically have their value synced to all connected client game instances. FishNet also has [Remote Procedure Calls](../features/network-communication/remote-procedure-calls.md), which are methods that you call on one game instance and FishNet will execute the method on a different game instance instead of executing it locally.

To enable these features, FishNet needs to be able to identify game objects and scripts across the network so that it can know which game object and specific script on that game object you called an RPC[^1] or changed a SyncVar[^2] on. FishNet does this by assigning IDs to selected GameObjects that you give a [NetworkObject ](../features/networked-gameobjects-and-scripts/networkobjects/)component to. It also assigns IDs to any component that inherits from [NetworkBehaviour](../features/networked-gameobjects-and-scripts/network-behaviour-guides.md), which is a MonoBehaviour that supports RPCs, SyncVars, and useful network methods and properties.

For NetworkObjects that can be instantiated during run-time, FishNet needs to store a prefab of them and assign an ID to the prefab so that it can instantiate that same one across the network when you spawn it on the server. This is handled with a prefab collection scriptable object called the [Spawnable Prefabs](../../fishnet-building-blocks/scriptableobjects/spawnableprefabs/).

***

## The server and client

FishNet is capable of running as a server and/or as a client, and they can be started and stopped independently of one another. When you start the server, FishNet will set various fields such as `IsServerStarted` as true in that game instance that is running as the server. The same applies when you start a client; when both are running FishNet will also set values such as `IsHostStarted`. These fields can be used to determine if the code is running in the game instance acting as a server, client, or both.

***

## Key concepts

* **NetworkManager:** This is the central hub of your FishNet application. It manages the networking lifecycle, including starting and stopping servers and clients, handling connections, and overseeing the spawning of networked objects.
* **NetworkObject:** Any GameObject in your Unity scene that needs to be linked across the network must have a NetworkObject component attached. This component assigns a unique network ID and manages the object's networked lifecycle, ensuring its existence and state are consistent across all connected clients.
* **NetworkBehaviour:** Similar to Unity's MonoBehaviour, NetworkBehaviour is the base class for all scripts that need to directly contain RPCs, SyncTypes, or NetworkBehaviour methods. Scripts inheriting from `NetworkBehaviour` can access useful properties like `NetworkManager`, `IsServerStarted`, `IsSpawned`, `IsOwner`, as well as be directly referenced across the network.
* **Ownership and Observers:** FishNet provides a clear system for defining **ownership** of NetworkObjects, typically assigned to the client that created or controls the object. **Observers** are clients that are "observing" a particular NetworkObject, meaning they receive updates about its state. This allows for efficient data transmission, sending updates only to relevant clients.
* **Remote Procedure Calls (RPCs):** RPCs are a core mechanism for communication between clients and the server. They allow you to call methods on a specific NetworkBehaviour instance that execute on a remote machines linked version of that instance. Any script can call an RPC, even regular MonoBehaviours, ScriptableObjects, or other classes.
* **SyncVars:** These are powerful features for automatically synchronizing variable values and complex data structures (like lists or dictionaries) across the network. When a SyncVar's value changes on the server, it's automatically replicated to all observing clients.

[^1]: Remote Procedure Call

[^2]: Synchronized Variable

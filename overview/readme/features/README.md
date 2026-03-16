# Features

{% hint style="warning" %}
For a more exact feature comparison of various networking solutions please see this [comparison chart](https://docs.google.com/spreadsheets/d/1Bj5uLdnxZYlJykBg3Qd9BNOtvE8sp1ZQ4EgX1sI0RFA/).
{% endhint %}

Performance and reliability are very important to us, but so is including great features to make your experience easier, and better. The last thing we want to do is create limitations; we support all features Mirror does, and then some.

Below is a comparison of unique and enhanced features, with more to come.

## Features unique to Fish-Networking

> **Update Stability**
>
> Other solutions do not guarantee that their API will not change between releases, making it difficult and dangerous to update for fixes or features. Fish-Networking prides itself on it's [No-Break Promise](../../../#no-break-promise), ensuring updates do not require alteration of user code, keeping you on-top with the best to offer.

> **Built-in Addressables Support**
>
> Unlike other networking solutions, Fish-Networking has addressables support included within core features. Addressable scenes are easily implemented with our network SceneManager, and addressable prefabs are supported as well.

> **Network Balancing**
>
> NetworkManagers are the center of your network environment. They contain other managers, and handle communications between the client and server. Fish-Networking allows an infinite number of NetworkManagers, allowing clients at once to connect to multiple different servers, to have each handle certain tasks or worlds. Various NetworkManagers may also be used to fit different game modes, transports, platforms, and a number of other tasks.

> **Local Remote Calls**
>
> Fish-Networking allows remote calls to run not only on their intended destination, but also on the caller as well. Typically you would have to copy logic into multiple methods to accomplish such a task, but Fish-Networking allows it to be done in one method. This drastically reduces potential errors in code, lines of code, and improves ease of use.

> **Multi-purpose Remote Calls**
>
> Target and Observers remote calls can be used interchangeable, in the same method. This allows you to use a single method to send data to one client or multiple clients. Like _**Local Remote Calls**_, this creates a more reliable code-base and improves the user experience.

> **Buffered Remote Calls**
>
> Often the last value of a remote call must be sent to newly joined clients. Fish-Networking is one of few solutions that offer this ability.

> **Client-side Prediction**
>
> Client-side prediction primarily focuses on ensuring players in your game cannot cheat a variety of movement types. This feature is an absolute must for any competitive game.
>
> Fish-Networking is the only free solution to provide built-in client-side prediction with only a few lines of code. Also, included are a variety of components for desynhronization smoothing, and rigidbody prediction.

> **Lag Compensation (pro feature)**
>
> Another very important feature for precision based gaming is lag compensation. This is the act of rolling back colliders in time to where a client had seen them; this ensures accurate hit registration. This technique is applicable to several genre types but is most commonly seen in shooter games.

> **Automatic Code Stripping (pro feature)**
>
> Code stripping helps protect your game server by removing sensitive logic that the player should not be aware of.

> **Large Packet Handling**
>
> In some scenarios you may need to send large amounts of data either to the client or server. Tests show Fish-Networking was able to effectively send anywhere from a few, to a few hundred megabytes over the network. Other solutions such as Mirror and even Fusion throw errors on the same job.

> **Reliable Order of Operations**
>
> Race conditions are possibly the worst kind of problems to solve, and it's especially frustrating when those race conditions are caused by someone else's software. Competitors such as Mirror have shown to have a variety of race conditions, such as networked objects referencing one-another before they actually exist.
>
> Fish-Networking ensures references cannot go missing, and that the reliability of operations and callbacks are consistent in all scenarios.

> **Child Network Components**
>
> Child Network Components allow for easier design and better code organization by using components such as Network Objects or Network Behaviours on child objects. Mirror for example requires all networked components to be placed on the root object, which could quickly clutter your work flow.

> **Custom SyncTypes**
>
> Custom SyncTypes allow you to create logic on how something may be updated over the network. For example: SyncVars, SyncList, SyncDictionaries are all custom SyncTypes included within Fish-Networking. If you would prefer a more hands-on/tailored experience you may create your own Custom SyncType using the exposed framework.

> **Dual Projects**
>
> Fish-Networking has limited support for dual projects. We recommend using a single project and utilize server defines along with our **automatic code stripping** feature.

> **Single, Additive, Stacked Scene Management**
>
> Scene management is essential for offline and online games. While other solutions such as Mirror, Fusion, and Netcode are limited to basic single scene management out of the box, Fish-Networking provides built-in logic to control seperation of clients through single, additive, and even stacked scenes. This is a powerful tool for world streaming, dungeon instances, lobbys and more.

> **Automatic Prefab Detection**
>
> Networking solutions such as Mirror require you to specify and organize networked object prefabs. This is a time consuming task which often requires dragging every prefab into a collection, or looking up prefabs by string before you may spawn them over the network. Fish-Networking strongly believes in ease-of-use, and part of that is having your networked prefabs automatically detected by our framework. Changes made to networked objects are automatically known and configured by Fish-Networking without requiring additional steps form the user.

> **Built-in, or Custom Object Pool**
>
> Included is the ability to automatically reset and pool network objects. Objects can be set to pool at runtime, or be pre-configured on the NetworkObject. Pooled objects automatically have their network values reset, including all SyncTypes on the object.
>
> Fish-Networking comes with a basic object pool but fully supports any object pool of your choice.

> **Zero Hotpath Allocations**
>
> Allocations can massively affect both server and client performance. While Mirror and Netcode allocate garbage regularly, Fish-Networking does not, ensuring better scalability and all-around performance.

> **Offline Networked Objects**
>
> In some instances an object may need to only exist locally, rather than over the network, and sometimes both. Fish-Networking allows users to default objects to being local only so that the objects network functionality is unused. In addition, these objects are intelligently synchronized over the network when the user shows such intent.

## Features which do more in Fish-Networking

> **Serializables**
>
> Fish-Networking supports more object types to be serialized than any other free solution. Dictionaries, scriptable objects, nullables and more can be automatically serialized in Fish-Networking. Solutions which have existed longer, such as Mirror, still require manual serialization of many types which Fish-Networking handles automatically.

> **Area of Interest System**
>
> Area of interest, often called AOI, controls which clients receive what information. AOI not only can prevent cheating by disallowing information clients get, but also drastically improve bandwidth and performance of the server by not sending unnecessary information to clients. Both Fish-Networking and Mirror support AOI, however in Mirror you may only use one AOI system at a time. For example, you can separate clients by scenes in Mirror, but you cannot also separate them by distance at the same time. In result if you have a larger world players will see each other at all times. Fish-Networking not only streamlines setting up the AOI system but allows multiple conditions to run at once, which would resolve the just mentioned Mirror problem. Fish-Networking's AOI system can also be easily customized with your own logic.

> **Reliable and Unreliable Remote Calls**
>
> Several networking solutions support reliable and unreliable remote calls, but Fish-Networking simplifies the process for users. In Mirror if you wish to have a remote call which can send both reliable and unreliable you must duplicate your logic into multiple methods, one for reliable and one for unreliable. Fish-Networking again saves on lines of code and risk of error by allowing a single remote call method to run both reliably and unreliably. You can change the reliability effortlessly at runtime as well.

> **SyncType Flexibility**
>
> [SyncTypes](../../../guides/features/network-communication/synchronizing/) are another very common feature found in just about every high level library. However, very few solutions offer flexibility on how SyncTypes can be configured. Mirror for example can only send SyncTypes reliably, cannot adjust the interval per SyncType, has limited callbacks, and cannot control who receives the values of each SyncType. Fish-Networking on the other hand may send reliable or unreliable with eventual consistency. SyncType intervals can be different for each SyncType. Callbacks can occur on both the server and client. Each SyncType can be specified to go to only the owner, or all clients.

> **Tick Based**
>
> Tick based timings allow accurate simulation for client-side prediction, smooth transform replication, and a lot more. Fish-Networking has a strong and efficient tick based core to provide a smoother experience for both the developer and their players. Older networking solutions such as Mirror are not tick based, which results in sloppy simulations as well the inability to create a proper client-side prediction system.

> **Broadcasts and Messages**
>
> Fish-Networking supports broadcasts, interchangeably called messages. Broadcasts and messages are data which can be sent over the network without requiring a networked object receiver. These are useful for objects which you don't want networked but would still like to be able to communicate with, such as a door in your scene. Fish-Networking enhances this communication type by allowing multiple objects to utilize the same message. For example, in Fish-Networking all your scene doors could listen to the same message for updates; Mirror on the other hand only one door could listen to updates while you would have to create additional broadcast or message types for every single added door.

> **Original**
>
> Mirror, Netcode, and several other networking solutions are based off from previous works, some which are deprecated. Fish-Networking is made original from the ground up, with no prior solution's limitations to bog it down.

> **Anti-Singleton Design**
>
> Most networking solutions, even newer ones such as Netcode, use a singleton design pattern. Fish-Networking does not utilize singleton classes for it's managers, but still provides an easy way to access networking data from anywhere.

> **XML Documentation**
>
> An often unconsidered part of any project are the XML comments for it's API. Netcode, Mirror, and even Unity's official API have several parts which are not fully commented internally nor through the XML. Fish-Networking provides complete XML coverage with well thought-out descriptions to make developing easier without constantly have to jump through the source code or visit external sources.

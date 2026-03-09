---
description: >-
  Sometimes logic has to be changed for the better, but it doesn't have to be a
  rough experience. You may find planned breaks here, and how to resolve them.
---

# Changes in API

## 4.1.0

**NetworkManager**

> **CanLog, Log, LogWarning, LogError**
>
> Errors may show that these methods no longer exist, they however do but with different usage.
>
> These methods have been moved under the namespace FishNet.Managing and are now called using either a NetworkManager reference, valid or null, or by using NetworkManagerExtensions.MethodName.

> **GetPooledInstantiated(GameObject, ushort, bool) is missing**
>
> Changed to GetPooledInstantiated(GameObject, bool).

> **GetPooledInstantiated(int, bool) is missing**
>
> Changed to GetPooledInstantiated(int, ushort, bool).

> **GetPooledInstantiated(NetworkObject, ushort, bool) is missing**
>
> Changed to GetPooledInstantiated(NetworkObject, bool).

> **StorePooledInstantiated(NetworkObject, int, bool) is missing.**
>
> Changed to StorePooledInstantiated(NetworkObject, bool).

> **Authenticator is missing.**
>
> Use ServerManager.GetAuthenticator or ServerManager.SetAuthenticator instead.

> **GetInstance(bool warn = true) is missing.**
>
> Use GetInstance() or TryGetInstance(T).

**TimeManager**

> **LastPacketTick**
>
> Changed from uint to EstimatedTick.
>
> You may see errors such as: cannot convert type 'uint' to 'EstimatedTick'.
>
> Use LastPacketTick.LastRemoteTick. Reviewing the EstimatedTick API is recommended to leverage it's features better.

#### Misc

**PooledReader and PooledWriter**

> **Dispose and DisposeLength are missing.**
>
> Use Store and StoreLength.

**ReaderPool and WriterPool**

> **GetReader, GetWriter have been renamed to be consistent with other API.**
>
> Changed to Retrieve.

> **Recycle has been renamed to be consistent with other API.**
>
> Changed to Store.

**Observer Conditions**

> **DistanceCondition.UpdateFrequently is missing.**
>
> Method no longer used.

> **ObserverCondition.Timed is missing.**
>
> Changed to GetConditionType().

> **ObserverCondition.Clone is missing.**
>
> Method no longer used. Use Awake to initialize custom conditions.

> **MatchCondition had several fields replaced. The following fields were removed:**
>
> * **MatchConnections**
> * Changed to GetMatchConnections(NetworkManager)
> * **ConnectionMatch**
> * Changed to GetConnectionMatches(NetworkManager)
> * **MatchObject**
> * Changed to GetMatchObjects(NetworkManager)
> * **ObjectMatch**
> * Changed to GetObjectMatches(NetworkManager)

**SyncTypes**

> **SyncVar and SyncObject attribute are no longer used.**
>
> Remove SyncVar/SyncObject attribute. When setting or getting the SyncVar's value be sure to use the `.Value` of it instead of trying to set or get it directly.

> **ISyncType, SyncBase.Reset is missing.**
>
> Changed to ResetState.

> **SyncBase.Read(PooledReader) is missing.**
>
> Changed to Read(PooledReader, bool).

> **SyncVar fields must be declared as SyncVar.**
>
> Fields previously identified as SyncVars using the SyncVar attributes must now use SyncVar\<Type> as the field, and must be initialized like SyncObjects.
>
> ```csharp
> // Example.
> private readonly SyncVar<float> _health = new();
>
> private void MyMethod()
> {
>     Debug.Log($"Current health is {_health.Value}");
>     // Set health to a new value.
>     _health.Value = 10f;
> }
> ```

**SceneUnloadEndEventArgs**

> **UnloadedSceneHandles, UnloadedSceneNames, UnloadedScenesV2 is missing.**
>
> Changed to UnloadedScenes.

**PredictedObject**

> **XYZMethod is missing.**
>
> A variety of methods within PredictedObject have been replaced by a simpler variant.
>
> The following methods were removed:
>
> * **GetSmoothTicks**
> * **GetInterpolation**
> * **SetInterpolation**
>
> The removed methods have been replaced with **SetSpectatorSmoothingType.**

**Misc**

> **Broadcast: cannot convert from 'method group'.**
>
> Broadcast handlers now require Channel as the last parameter.
>
> ```csharp
> // Example:
> // Before
> private void OnResponseBroadcast(ResponseBroadcast rb)
> // After
> private void OnResponseBroadcast(ResponseBroadcast rb, Channel channel)
> ```

> **IsClient, IsServer, IsHost, IsServerOnly, IsClientOnly are Obsolete.**
>
> These fields return if the socket is started and for many those intentions were not clear. In result all instances have been replaced with Started versions of each; eg: IsClientStarted.
>
> To check if an object is initialized for the server or client use Initialized varients, such as IsClientInitialized.

> **SceneLoad/UnloadData PreferredScene change.**
>
> PreferredScene has been changed to a structure to allow setting separate preferred scenes for server and client.

> **NetworkAnimator.ForceSend is missing.**
>
> Method no longer used. NetworkAnimator.SendAll may be of use.

> **NetworkConnection.Tick is missing.**
>
> Changed to NetworkConnection.LocalTick

> **ServerObjects RebuildObservers method is missing.**
>
> All IEnumerable RebuildObservers methods have been replaced with IList variants.

> **ServerManager.Authenticator is missing.**
>
> Changed to GetAuthenticator and SetAuthenticator.

> **NetworkBehaviour.DirtySyncTypes is missing.**
>
> Method no longer used.

> **NetworkObject.ResetForObjectPool is missing.**
>
> Changed to ResetState.

> **NetworkObject.ClientInitialized is missing.**
>
> Changed to NetworkObject.IsClientInitialized.

> **Replicate and Reconcile attribute Resends is missing.**
>
> Replaced with PredictionManager.RedundancyCount.

> **Rollback(PreciseTick, PhysicsType, bool asOwner)**
>
> Changed to Rollback(PreciseTick, RollbackPhysicsType, bool)

> **ReadDictionary\<TKey, TValue>()**
>
> Changed to ReadDictionaryAllocated\<TKey, TValue>()

> **ListCache\<T>, ListCaches is missing**
>
> Classes replaced with better versions under a different namespace.
>
> The following are their replacements:
>
> * CollectionCaches
> * ResettableCollectionCaches
> * ObjectCaches
> * ResettableObjectCaches

> **RetrieveObject(int, bool) is missing.**
>
> Changed to RetrieveObject(int, ushort, bool).

## 3.3.0

Unscheduled break.

> **ObserversRpc IncludeOwner is missing.**
>
> Changed to ExcludeOwner. **Remeber to flip the values.**

## 3.1.1

**Prediction Changes**

Replicate datas must now implement _IReplicateData_, and Reconcile datas must implement _IReconcileData_.

As of this release prediction methods can now have their Channels set, so you may ensure certain messages never drop. In result your replicate method signatures should now be as shown below.

```csharp
private void MyReplicate(ReplicateData rd, bool asServer, Channel channel = Channel.Unreliable, bool replaying = false)
```

Reconcile methods must also now include a Channel parameter as shown.

```csharp
private void MyReconcile(ReconcileData rd, bool asServer, Channel channel = Channel.Unreliable)
```

For more detailed instructions on these changes see the [prediction guide](../features/prediction/).

**TimeManager Fields and Events**

Many prediction related events and values were moved out of the _TimeManager_ and placed into the _PredictionManager_. Use PredictionManager.XYZ instead of TimeManager.XYZ.

The following events and fields were moved.

* LastReconcileTick.
* LastReplicateTIck.
* IsReplaying(), IsReplaying(Scene).
* OnPreReconcile.
* OnPostReconcile.
* OnPreReplicateReplay.
* OnPostReplicateReplay.

In addition two events had their signatures changed. OnPre/PostReplicateReplay now also includes the tick being replayed.

> **Changed Signature OnPre/PostReplicateReplay(PhysicsScene, PhysicsScene2D).**
>
> Replaced with OnPre/PostReplicateReplay(uint, uint).

**Misc**

> **Changed LoggingConfiguration is now a base class.**
>
> LoggingConfiguration can be used to create custom loggers. _LevelLoggingConfiguration_ is now the default logger. If you were previously using a LoggingConfiguration with custom settings create new as a LevelLoggingConfiguration.

> **Changed Signature NetworkConnection.LoadedStartScenes.**
>
> Replaced with NetworkConnection.LoadedStartScenes().

> **Removed RigidbodyPauser.SetTimeManager.**
>
> Replaced with RigidbodyPauser.SetPredictionManager; now takes PredictionManager argument.

> **Removed NetworkBehaviour/NetworkObject.Deinitializing.**
>
> Replaced with NetworkBehaviour/NetworkObject.IsDeinitializing.

> **Removed NetworkObject.SceneObject.**
>
> Replaced with NetworkObject.IsSceneObject.

> **Removed ServerOnlyCondition.**
>
> Replaced with with OwnerOnlyCondition.

> **Removed TimeManager.TickPercent.**
>
> Replaced with TimeManager.GetPreciseTick or TimeManager.GetTickPercent.

> **Removed TimeManager.TicksToTime(bool).**
>
> Replaced with TimeManager.TicksToTime(TickType).

> **Removed TimeManager.OnPhysicsSimulation.**
>
> Replaced with TimeManager.OnPrePhysicsSimulation.

> **Removed NetworkManager.Authenticator.**
>
> Replaced with ServerManager.Get/SetAuthenticator.

> **Removed ServerManager.Authenticator.**
>
> Replaced with ServerManager.Get/SetAuthenticator.

> **Changed Signature Transport.GetServerBindAddress.**
>
> Replaced with Transport.GetServerBindAddress(IPAddressType).

> **Changed Signature Transport.SetServerBindAddress(string).**
>
> Replaced with Transport.SetServerBindAddress(string, IPAddressType).

> **Removed ListCaches.XYZCache.**
>
> Replaced with ListCaches.GetXYZCache, ListCaches.SetXYZCache.

## 2.0.0

**Misc**

> **Removed NetworkBehaviour/NetworkObject.OwnerIsValid.**
>
> Replaced with NetworkBehaviour/NetworkObject.Owner.IsValid.

> **Removed NetworkBehaviour/NetworkObject.OwnerIsActive.**
>
> Replaced with NetworkBehaviour/NetworkObject.Owner.IsActive.

> **Changed Name LoadOptions.DisallowStacking.**
>
> Change references from DisallowStacking to AllowStacking and inverse value.

> **Changed Name Several enums are being changed from plural to singular. Those changed will be SceneScopeTypes, ServerUnloadModes, LocalConnectionStates, RemoteConnectionStates.**
>
> Remove plural (s) from each enum reference. EG: SceneScopeTypes will become SceneScopeType.

> **Removed TimeManager.TicksToTimeDouble.**
>
> Replaced with TimeManager.TicksToTime; now returns a double.

> **Changed Signature Transport.Initialize(networkManager).**
>
> Add int as a second parameter to your transport.Initialize method. Include Transport.Index within the following callback structures: ClientConnectionStateArgs, ServerConnectionStateArgs, RemoteConnectionStateArgs, ClientReceivedDataArgs, ServerReceivedDataArgs.

> **Removed Transport.GetChannelCount(), Transport.GetDefaultReliableChannel(), Transport.GetDefaultUnreliableChannel().**
>
> Delete call or overrides to methods. There will always only be two channel counts, 0 for reliable and 1 for unreliable.

> **Removed Transport.GetDefaultReliableChannel() and GetDefaultUnreliableChannel().**
>
> Delete calls or overrides to methods. Channel.Reliable should always be id 0, and Channel.Unreliable id 1.

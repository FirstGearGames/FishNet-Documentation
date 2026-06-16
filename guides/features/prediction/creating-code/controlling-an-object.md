---
description: Learn how to create a predicted object that the owner or server can control.
---

# Controlling an Object

## Data structures

Implementing prediction is done by creating a replicate and reconcile method, and making calls to the methods accordingly.

Your replicate method will take inputs you wish to run on the owner, server, and other clients if using state forwarding. This would be any input needed for your controller such as jumping, sprinting, movement direction, and could even include other mechanics such as holding a fire button.

The reconcile method takes a state of the object after a replicate is performed. This state is used to make corrections in the chance of de-synchronizations. For example, you may send back health, velocity, transform position and rotation, so on.

It's also worth mentioning if you are going to allocate in your structures it could be beneficial to utilize the Dispose callback, which will run as the data is being discarded.

Here are the two structures containing basic mechanics for a rigidbody.

```csharp
public struct ReplicateData : IReplicateData
{
    public bool Jump;
    public float Horizontal;
    public float Vertical;
    public ReplicateData(bool jump, float horizontal, float vertical) : this()
    {
        Jump = jump;
        Horizontal = horizontal;
        Vertical = vertical;
    }

    private uint _tick;
    public void Dispose() { }
    public uint GetTick() => _tick;
    public void SetTick(uint value) => _tick = value;
}

public struct ReconcileData : IReconcileData
{
    // PredictionRigidbody is used to synchronize rigidbody states
    // and forces. This could be done manually but the PredictionRigidbody
    // type makes this process considerably easier. Velocities, kinematic state,
    // transform properties, pending velocities and more are automatically
    // handled with PredictionRigidbody.
    public PredictionRigidbody PredictionRigidbody;
    
    public ReconcileData(PredictionRigidbody pr) : this()
    {
        PredictionRigidbody = pr;
    }

    private uint _tick;
    public void Dispose() { }
    public uint GetTick() => _tick;
    public void SetTick(uint value) => _tick = value;
}

```

{% hint style="info" %}
Learn more about using [PredictionRigidbody](../predictionrigidbody.md).
{% endhint %}

## Preparing to call prediction methods

Typically speaking you would want to run your replicate(or inputs) during OnTick. When you send the reconcile depends on if you are using physics bodies or not.

When using physics bodies, such as a rigidbody, you would send the reconcile during OnPostTick because you want to send the state after the physics have simulated your replicate inputs. See the [TimeManager API ](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Timing.TimeManager.html#FishNet_Managing_Timing_TimeManager_OnPostTick)for more details on tick and physics event callbacks.

Non-physics controllers can also send in OnTick, since they do not need to wait for a physics simulation to have the correct outcome after running inputs.

The code below shows which callbacks and API to use for a rigidbody setup.

{% hint style="info" %}
You may need to modify move and jump forces depending on the shape, drag, and mass of your rigidbody.
{% endhint %}

```csharp
// How much force to add to the rigidbody for jumps.
[SerializeField]
private float _jumpForce = 8f;
// How much force to add to the rigidbody for normal movements.
[SerializeField]
private float _moveForce = 15f;
// PredictionRigidbody is set within OnStart/StopNetwork to use our
// caching system. You could simply initialize a new instance in the field
// but for increased performance using the cache is demonstrated.
public PredictionRigidbody PredictionRigidbody;
// True if to jump next replicate.
private bool _jump;

private void Awake()
{
    PredictionRigidbody = ObjectCaches<PredictionRigidbody>.Retrieve();
    PredictionRigidbody.Initialize(GetComponent<Rigidbody>());
}
private void OnDestroy()
{
    ObjectCaches<PredictionRigidbody>.StoreAndDefault(ref PredictionRigidbody);
}
public override void OnStartNetwork()
{
    base.TimeManager.OnTick += TimeManager_OnTick;
    base.TimeManager.OnPostTick += TimeManager_OnPostTick;
}

public override void OnStopNetwork()
{
    base.TimeManager.OnTick -= TimeManager_OnTick;
    base.TimeManager.OnPostTick -= TimeManager_OnPostTick;
}
```

{% hint style="info" %}
Instead of needing to manually subscribe to the TimeManager events such as OnTick and OnPostTick you can have your class inherit from [TickNetworkBehaviour](../../../../fishnet-building-blocks/components/network-behaviour-components/ticknetworkbehaviour.md) and then use its override methods.
{% endhint %}

## Calling prediction methods

For our described demo, below is how you would gather input for your replicate and reconcile methods.

Update is used to gather inputs which are only fired for a single frame. Ticks do not occur every frame, but rather at the interval of your TickDelta, much like FixedUpdate works. While the code below only uses Update for single frame inputs there is nothing stopping you from using it for held inputs as well.

```csharp
private void Update()
{
    if (base.IsOwner)
    {
        if (Input.GetKeyDown(KeyCode.Space))
            _jump = true;
    }
}
```

OnTick will now be used to build our replicate data. A separate method of 'CreateReplicateData' is not needed to create the data but is done to organize our code better.

When attempting to create the replicate data we return with default if not the owner of the object. Server receives and runs inputs from the owner so it does not need to create data, and when clients do not own an object they will get the input for it from the server, as forwarded by other clients if using state forwarding. When not using state forwarding default should still be used in this scenario, but clients will not run replicates on non-owned objects. You can also run inputs on the server if there is no owner; using base.HasAuthority would probably be best for this. See [Checking Ownership](../../ownership/#checking-ownership) for more information.

```csharp
private void TimeManager_OnTick()
{
    RunInputs(CreateReplicateData());
}

private ReplicateData CreateReplicateData()
{
    if (!base.IsOwner)
        return default;

    // Build the replicate data with all inputs which affect the prediction.
    float horizontal = Input.GetAxisRaw("Horizontal");
    float vertical = Input.GetAxisRaw("Vertical");
    ReplicateData md = new ReplicateData(_jump, horizontal, vertical);
    _jump = false;

    return md;
}
```

Now implement your replicate method. The name may be anything but the parameters shown are required. The first is what we pass in, the remainder are set at runtime. Although, you absolutely may change the default channel used in the parameter or even at runtime.

For example, it could be beneficial to send an input as reliable if you absolutely want to ensure it's not dropped due to network issues.

```csharp
[Replicate]
private void RunInputs(ReplicateData data, ReplicateState state = ReplicateState.Invalid, Channel channel = Channel.Unreliable)
{
    /* ReplicateState is set based on if the data is new, being replayed, ect.
    * Visit the ReplicationState enum for more information on what each value
    * indicates. At the end of this guide a more advanced use of state will
    * be demonstrated. */
    
    // Be sure to always apply and set velocities using PredictionRigidbody
    // and never on the rigidbody itself; this includes if also accessing from
    // another script.
    Vector3 forces = new Vector3(data.Horizontal, 0f, data.Vertical) * _moveRate;
    PredictionRigidbody.AddForce(forces);

    if (data.Jump)
    {
        Vector3 jmpFrc = new Vector3(0f, _jumpForce, 0f);
        PredictionRigidbody.AddForce(jmpFrc, ForceMode.Impulse);
    }
    // Add gravity to make the object fall faster. This is of course
    // entirely optional.
    PredictionRigidbody.AddForce(Physics.gravity * 3f);
    // Simulate the added forces.
    // Typically you call this at the end of your replicate. Calling
    // Simulate is ultimately telling the PredictionRigidbody to iterate
    // the forces we added above.
    PredictionRigidbody.Simulate();
}
```

{% hint style="info" %}
On non-owned objects a number of replicates will arrive as ReplicateState Created, but will contain default values. This is our PredictionManager.RedundancyCount feature working.

This is normal and indicates that the client or server had gracefully stopped sending states as there is no new data to send. This can be useful if you are [Predicting States.](understanding-replicatestate/predicting-states-in-code.md)
{% endhint %}

Now the reconcile must be sent to clients to perform corrections. Only the server will actually send the reconcile but be sure to call CreateReconcile no matter if client, server, owner or not; this is to future proof an upcoming feature. Unlike our CreateReplicateData method, using CreateReconcile is not optional.

```csharp
private void TimeManager_OnPostTick()
{
    CreateReconcile();
}

// Create the reconcile data here and call your reconcile method.
public override void CreateReconcile()
{
    // We must send back the state of the rigidbody. Using your
    // PredictionRigidbody field in the reconcile data is an easy
    // way to accomplish this. More advanced states may require other
    // values to be sent; this will be covered later on.
    ReconcileData rd = new ReconcileData(PredictionRigidbody);
    // Like with the replicate you could specify a channel here, though
    // it's unlikely you ever would with a reconcile.
    ReconcileState(rd);
}

```

Reconciling only a rigidbody state is very simple.

```csharp
[Reconcile]
private void ReconcileState(ReconcileData data, Channel channel = Channel.Unreliable)
{
    // Call reconcile on your PredictionRigidbody field passing in
    // values from data.
    PredictionRigidbody.Reconcile(data.PredictionRigidbody);
}
```

{% hint style="info" %}
When reconciling an object with a CharacterController, ensure you disable the CharacterController component immediately before setting its position and re-enable it afterwards. Because this controller uses a custom physics engine, skipping this toggle will prevent the internal state from refreshing, resulting in a synchronization error between the physics and the transform.\
As an example:<br>

```csharp
_characterController.enabled = false;
transform.localPosition = reconcileData.Position;
_characterController.enabled = true;
```
{% endhint %}

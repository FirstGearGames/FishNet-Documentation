---
description: >-
  The PredictionManager provides states, callbacks, and settings to fine tuning
  prediction for your game type.
---

# PredictionManager

## Description <a href="#server-and-host" id="server-and-host"></a>

The **PredictionManager** is a core component responsible for managing the prediction system, which allows clients to predict their own actions and then reconcile (correct) their state when authoritative updates are received from the server. It allows you to tweak various global prediction settings and get certain prediction callbacks.

{% hint style="success" %}
Check out its API page for more specific methods and events [here](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Predicting.PredictionManager.html).
{% endhint %}

## Settings <a href="#server-and-host" id="server-and-host"></a>

<div align="left"><figure><img src="../../../.gitbook/assets/prediction-manager-component.png" alt=""><figcaption><p>Default settings</p></figcaption></figure></div>

### :gear: Create Local States

> When enabled this allows the client to create local reconcile states which means reconciles can be sent less frequently and provide data to use for reconciles when packets are lost.

### :gear: State **Interpolation**

> This is how many states to try and hold in a buffer before running them on clients. Larger values add resilience against network issues at the cost of running states later.

### :gear: State **Order**

> The order in which clients run states. **Future** favors performance and does not depend upon reconciles, while **Past** favors accuracy but clients must reconcile every tick.

### :gear: **Drop Excessive Replicates**

> Toggle the setting to true to drop excessive client replicates, helping prevent attacks but risking temporary desynchronization during connectivity issues. Set to false to store up to 3 seconds of replicates, processing multiple per tick for quicker cache clearing. This approach ensures all inputs are executed but could enable speed hacking. When false, replicated data is consumed more quickly if the cache limit is exceeded.

### :gear: Maximum Server Replicates

> This is shown when the **Drop Excessive Replicates** is enabled. This value indicates the maximum number of replicates which the server will cache from client before dropping old ones. Higher values will reduce the chance of dropped input when the client's connection is unstable, but will potentially add latency to the client's object both on the server and client. Generally, cached replicates will never significantly exceed Queued Inputs.

---
description: >-
  The DebugManager helps you debug various networking functions when in the
  Unity Editor or Development builds.
---

# DebugManager

## Description <a href="#server-and-host" id="server-and-host"></a>

The **Debug Manager** component can be used to gain extra information and temporarily tweak certain networking aspects to assist in debugging errors in your game. For example, if you are running into Scene ID errors you can enable the Detailed Writing of Scene Objects to know the object names instead of just IDs. These settings often come at the cost of a little more bandwidth used and are only enabled in the Editor and Development builds of your game.

{% hint style="warning" %}
Note that having a DebugManager with some of its settings enabled can cause issues when connecting non-development builds to development ones (including the editor).
{% endhint %}

## Settings <a href="#server-and-host" id="server-and-host"></a>

<div align="left"><figure><img src="../../../.gitbook/assets/debug-manager-component.png" alt=""><figcaption><p>Default settings</p></figcaption></figure></div>

### :gear: **Scene Objects**

> This adds information about scene objects instead of just sending IDs. You can use this when debugging missing sceneId / spawn-related issues.

### :gear: **RPC Lengths**

> This will validate written versus read length of RPCs when enabled. Use in development to detect serialization mismatches in RPCs (written length vs read length).

### :gear: **ObserversRpcs**

> This disables ObserverRpc links.

### :gear: **TargetRpcs**

> This disables TargetRpc links.

### :gear: **ServerRpcs**

> This disables ServerRpcs links.

### :gear: **ReplicateRpcs**

> This disables ReplicateRpc links.

### :gear: **ReconcileRpcs**

> This disables ReconcileRpc links.

---
description: >-
  NetworkBehaviour allows your scripts to perform networked actions, such as
  synchronizing data, responding to network events, and handling remote
  procedure calls (RPCs).
---

# NetworkBehaviour

## Description

NetworkBehaviour(s) cannot be added to objects directly. This script must be inherited from before it may be included on an object. Inheriting from NetworkBehaviour allows access to [network communications](../../../guides/high-level-overview/terminology/communicating.md) and other useful data about the network, server, and client.

Each NetworkBehaviour must be attached to a GameObject that also has a NetworkObject component on it or one of its parent objects.

{% hint style="danger" %}
Adding a NetworkBehaviour derived component at run-time is not yet supported.
{% endhint %}

Please check out the guide page for more information about all the things you can do with NetworkBehaviours:

<table data-view="cards"><thead><tr><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><span data-gb-custom-inline data-tag="emoji" data-code="1f4d8">📘</span> <mark style="color:blue;">NetworkBehaviour Guide</mark></td><td><a href="../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md">network-behaviour-guides.md</a></td></tr><tr><td align="center">🔌 <mark style="color:yellow;">NetworkBehaviour API</mark></td><td><a href="https://fish-networking.com/FishNet/api/api/FishNet.Object.NetworkBehaviour.html">https://fish-networking.com/FishNet/api/api/FishNet.Object.NetworkBehaviour.html</a></td></tr></tbody></table>

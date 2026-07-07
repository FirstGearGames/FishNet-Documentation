---
description: >-
  This page discusses the various networking models and architectures used in
  games and what Fish-Networking uses.
---

# Networking Models

## Client-Server architecture

The **client-server model** is the most widely used approach in multiplayer networking. In this model:

* **Server**: The authoritative instance of the game. It manages game logic, synchronizes game state, validates player actions, and handles communication between clients.
* **Client**: A player's instance of the game. It sends input to the server and receives updates about the game state.

There are two main types of servers:

### **Dedicated server**

The server runs as its own process, separate from any player. It’s more secure and scalable, commonly used in competitive or large-scale games.

<figure><img src="../../.gitbook/assets/client-server-model.svg" alt="An image showing multiple clients connected to a single server" width="563"><figcaption><p>A dedicated server model</p></figcaption></figure>

As you can see from the above diagram, the clients (players) are all connected directly to the server and communicate with it instead of with each other.

***

### **Host (listen-server)**

The server and one of the clients run in the same process. This is common in smaller games or development environments.

<figure><img src="../../.gitbook/assets/listen-server-model.svg" alt="An image showing multiple clients connected to a single server which is also acting as a client itself" width="563"><figcaption><p>A listen-server model</p></figcaption></figure>

The above diagram demonstrates how the listen-server is similar to the dedicated server model where all clients connect directly with the server, but unlike the dedicated server, the listen server is also acting as a client (player).

{% hint style="success" %}
FishNet supports both dedicated and listen-server models out of the box.
{% endhint %}

***

## Peer-to-peer (P2P)

In **peer-to-peer networking**, each player (or peer) connects directly to others without a central server. This reduces latency and server costs but comes with challenges:

* Poor security and cheating prevention
* Difficult to synchronize game state reliably
* NAT traversal issues

<figure><img src="../../.gitbook/assets/p2p-model.svg" alt="An image of four clients all interconnected with each other." width="563"><figcaption><p>A peer-to-peer model</p></figcaption></figure>

P2P is rarely used in modern real-time multiplayer games, especially competitive ones, due to these drawbacks. FishNet does not use P2P, as it is designed around the client-server model for reliability and security.

{% hint style="info" %}
Very often a listen server setup is referred to as P2P, FishNet does support a listen server.
{% endhint %}

***

## Relay servers

Sometimes clients are unable to connect directly to the server due to **NAT (Network Address Translation)** or **firewall** restrictions. A **relay server** acts as a middleman to forward traffic between clients and the server.

<figure><img src="../../.gitbook/assets/relay-server-model.svg" alt="An image of a three clients connecting to a server which links them to a server and client instance." width="563"><figcaption><p>A relay server</p></figcaption></figure>

Relay servers:

* Allow connectivity when direct connections fail
* Introduce additional latency
* May be used as a fallback or for web-based games

{% hint style="success" %}
Fish-Networking supports multiple third-party relay services including [Unity's Relay](https://unity.com/products/relay), [Edgegap's Distributed Relay](https://edgegap.com/platform/distributed-relay), [Photon Realtime](https://www.photonengine.com/Realtime), [Epic Online Services' Relay Service](https://dev.epicgames.com/docs/epic-online-services/eos-overview#peer-to-peer-p2p-connections), and [Steam Datagram Relay](https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay).

There is also the free self-hosted [BlitzRelay](https://github.com/abdelfattahradwan/blitz-relay) that you can use.
{% endhint %}

***

### Host migration

When using a listen server, the host leaving the game would cause all players to leave the game; **Host migration** is a feature for maintaining game sessions in the event the current host disconnects or experiences network issues. It enables a new host to be elected from the remaining connected players, minimizing disruption and ensuring the game session can continue seamlessly.

{% hint style="warning" %}
Fish-Networking doesn't have built-in host migration, but it is possible (though tricky) to create it yourself.
{% endhint %}

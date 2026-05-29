---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# FishyUnityTransport

## General

A transport to utilize Unity's Official Transport with FishNet. Supports both Unity Transport 1.0 and Unity Transport 2.0\
\
Using **FishyUnityTransport** also allows you to utilize some of [Unity's Multiplayer Services](https://unity.com/solutions/multiplayer), like [Relay](https://docs.unity.com/en-us/relay/get-started)!

***

## Compatibility

<table data-full-width="false"><thead><tr><th width="149">System</th><th width="198">Supported?</th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Web</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>IOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Android</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Xbox</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:green;">Fully Supported</mark></td></tr></tbody></table>

***

## How to install

For installation instructions and usage please visit the official GitHub site.

Main: [https://github.com/ooonush/FishyUnityTransport](https://github.com/ooonush/FishyUnityTransport)

***

## Component settings

{% hint style="warning" %}
Settings marked in <mark style="color:yellow;">**Yellow**</mark> are only supported on Unity Transport 2.0!
{% endhint %}

* **Protocol Type:** Configure whether the transport is done via direct connection or over Unity Relay.
* <mark style="color:yellow;">**Use Web Sockets:**</mark> Per default the client/server will communicate over UDP. Set to true to communicate with WebSocket.
* <mark style="color:yellow;">**Use Encryption:**</mark> Per default the client/server communication will not be encrypted. Select true to enable DTLS for UDP and TLS for Websocket
* **Max Packet Queue Size:** The maximum amount of packets that can be in the internal send/receive queues. Basically this is how many packets can be sent/received in a single update/frame.
* **Max Payload Size:** The maximum size of an unreliable payload that can be handled by the transport.
* **Heartbeat Timeout (ms):** Timeout in milliseconds after which a heartbeat is sent if there is no activity.
* **Connect Timeout (ms):** Timeout in milliseconds indicating how long we will wait until we send a new connection attempt.
* **Max Connect Attempts:** The maximum amount of connection attempts we will try before disconnecting.
* **Disconnect Timout (ms):** Inactivity timeout after which a connection will be disconnected. The connection needs to receive data from the connected endpoint within this timeout. Note that with heartbeats enabled, simply not sending any data will not be enough to trigger this timeout (since heartbeats count as connection events
* **Connection Data:**
  * **Address:** IP address of the server (address to which clients will connect to)
  * **Port:** UDP port of the server.
  * **Server Listen Address:** IP address the server will listen on. If not provided, will use localhost.
* **Maximum Clients**: The Number of Clients allowed to connect to the FishNet server.

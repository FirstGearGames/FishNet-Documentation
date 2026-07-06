---
description: Transports control how data is sent, received, and handled over the network.
---

# Transports

The **Transport** is the low-level layer responsible for sending and receiving raw data over the network. It abstracts away the details of TCP/UDP sockets and handles packet delivery, reliability, ordering, etc.

FishNet uses events internally to plug into transport messages. Although it be unlikely you would need to access such messages, they are available to you for your development as well.

There are many transports available, some are maintained by the Fish-Networking team and others maintained by the community.

* [Tugboat](../../fishnet-building-blocks/transports/tugboat.md) (LiteNetLib)
* [Bayou](../../fishnet-building-blocks/transports/bayou.md) (WebGL)
* [Yak](../../fishnet-building-blocks/transports/yak-pro-feature.md) (for offline gameplay)
* [Multipass](../../fishnet-building-blocks/transports/multipass.md) (multi-transport support)
* [FishyBlitzRelay](../../fishnet-building-blocks/transports/fishyblitzrelay.md) (self-hosted relay)
* [FishySteamworks](../../fishnet-building-blocks/transports/fishysteamworks.md) (Steamworks.NET)
* [FishyFacepunch](../../fishnet-building-blocks/transports/fishyfacepunch-steam.md) (Facepunch for Steam)
* [FishyEOS](../../fishnet-building-blocks/transports/fishyeos-epic-online-services.md) (Epic Online Services)
* [FishyUnityTransport](../../fishnet-building-blocks/transports/fishyunitytransport.md) (Unity Transport)
* [FishyRealtime](../../fishnet-building-blocks/transports/fishyrealtime.md) (Photon Realtime)
* [FishyWebRTC](../../fishnet-building-blocks/transports/fishywebrtc.md) (WebRTC)
* [CanoeWebRTC](../../fishnet-building-blocks/transports/canoe-webrtc.md) (WebRTC)

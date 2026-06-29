---
description: >-
  Overview of key server hosting terms, comparing session-based vs persistent
  architectures, and outlining the roles of relays and dedicated servers.
---

# Terminology

### Session-based versus persistent

A session-based service, be it relay or dedicated server, will start up and end as needed. Given that these servers are on-demand(session-based) you typically pay-per-use rather than a flat rate, which can save you a substantial amount of money. Session-based services are the most common way online games are hosted and are typically used for matches and lobby based gaming, but can also be used for entire worlds with hundreds of players.

Persistent services are typically always running, no matter the number of players present. Because the servers are always available, this makes it easier to maintain the state of your worlds. A persistent server is the common choice for MMO-like games. While a constant world is very appealing this type of service often comes with significantly higher costs.

### Dedicated servers

A dedicated server refs to a server-authoritative game server. There are several advantages to a dedicated server, and very few cons.

**Benefits of a dedicated server:**

* **Scalability**: dedicated servers are rarely limited to a set number of players. If you need worlds which support dozens — even hundreds — of players, generally a dedicated server is the recommended route.
* **Security**: a dedicated server is the only way to effectively prevent player cheating, as the server becomes your game's authority and allows you to validate player information.
* **Bandwidth**: data transfer is rarely limited with dedicated servers. Relays may limit how much total data you can send, or throttle how frequently you can send data.
* **Latency:** for games which require a fast response time a dedicated server is mandatory. These servers can deploy across the world, providing the best latency and most neutrality for players.
* **Persistence**: if you want players to be able to join and leave servers, whether the service is session-based or persistent, a dedicated server will provide a smoother experience. Unlike peer-to-peer hosting, no player-host is required.

**Drawbacks of a dedicated server:**

* **Cost:** dedicated servers can be more expensive, especially if a free relay service is available through your platform or other means.

### Relays

A relay is a client-authoritative service, where the client acts as the server.

**Benefits of a relay:**

* **Cost-effective for Casual Games**: Relays are Ideal for casual or small-scale games, especially when a free relay service is available. However, using a paid relay service can quickly become impractical, as pay-per-use dedicated servers are often more affordable, while also gaining other advantages.
* **Self-hosting**: You can self-host a relay server which is far less demanding, and thus a lot cheaper, than hosting a full dedicated game server that runs all the game logic.

**Drawbacks of a relay:**

* **Scalability**: relays cannot scale. You are often limited to a number of players per session.
* **Security**: the client acting as the host is the authoritative source. Because there is not a neutral authority, cheating is very possible and often easily performed on relay servers. If cheating is a concern, a relay is likely not for you.
* **Bandwidth**: while total bandwidth usage is rarely restricted, most relay services impose limitations on data transfer frequency or size. Games that require rapid data transmission or large data chunks may experience throttling, leading to various performance issues.
* **Latency**: the hosting client's network connection determines latency for the entire session. A weak connection results in higher latency for all players, and players located far from the host will experience additional delay.
* **Persistence**: relay services do not support state persistence. When the host ends the session, all progress is lost. Additionally, if the host leaves unexpectedly, the game session typically terminates immediately for all connected players.

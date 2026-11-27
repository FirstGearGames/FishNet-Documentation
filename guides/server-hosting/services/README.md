---
description: >-
  A comparison of third-party hosting and relay services compatible with
  FishNet, including our recommended providers.
---

# Services

{% hint style="warning" %}
We strongly recommend reviewing our [Server Hosting](../) page for terminology as well the Pros/Cons of services mentioned on this page.
{% endhint %}

This information is updated to the best of our knowledge. If you are interested in a service listed below — to better understand their platform — please visit the provided links to their official website.

## Our recommended service

**PlayFlow Cloud**, a company we've had the pleasure of working with firsthand, is recommended as our top-pick for a hosting service. Their outstanding support, reliable service, and superior pricing have consistently impressed our staff and community.

<table data-card-size="large" data-view="cards" data-full-width="false"><thead><tr><th></th><th data-type="rating" data-max="5"></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>PlayFlow Cloud</strong></td><td>5</td><td><p><img src="../../../.gitbook/assets/attach_money_24dp_FFFFFF_FILL0_wght400_GRAD0_opsz24.png" alt="">Generous free tier with better pricing than competitors.</p><p><img src="../../../.gitbook/assets/group_24dp_FFFFFF_FILL0_wght400_GRAD0_opsz24.png" alt=""> Lobby &#x26; Matchmaking, with an included SDK handling the work.</p><p><img src="../../../.gitbook/assets/public_24dp_FFFFFF_FILL0_wght400_GRAD0_opsz24.png" alt=""> Worldwide Dedicated and instanced servers.</p><p><img src="../../../.gitbook/assets/graph_5_24dp_FFFFFF_FILL0_wght400_GRAD0_opsz24.png" alt=""> User-friendly control panel, metrics, support, and more.</p></td><td><a href="../../../.gitbook/assets/playflow-card.png">playflow-card.png</a></td><td><a href="playflow-cloud/">playflow-cloud</a></td></tr></tbody></table>

***

## Other services

Here is a non-extensive selection of other hosting services that work well with FishNet.

### Dedicated servers

These hosts offer session-based servers, as well persistent servers. Relay services may be available as well.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="image">Cover image</th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><ul><li>Pay-per-use pricing.</li><li>No egress fees.</li><li>Session-based servers available.</li><li>Persistent servers possible.</li></ul></td><td><a href="../../../.gitbook/assets/gameye-card.png">gameye-card.png</a></td><td><a href="https://gameye.com/">https://gameye.com/</a></td></tr><tr><td><ul><li>Pay-per-use pricing.</li><li>Session-based servers available.</li><li>Persistent servers available.</li><li>Relay servers available.</li><li>Match Making / Lobbies.</li></ul></td><td><a href="../../../.gitbook/assets/edgegap-card.png">edgegap-card.png</a></td><td><a href="https://edgegap.com/">https://edgegap.com/</a></td></tr><tr><td><p>AWS is a bit more difficult to setup and manage. We generally only recommend AWS to experienced developers.</p><ul><li>Pay-per-use pricing — a little more complex.</li><li>Session-based servers available.</li><li>Persistent servers available.</li><li>Relay servers available.</li><li>Match Making / Lobbies.</li></ul><p>We have a community guide for AWS: <a href="getting-started-with-aws.md" class="button primary" data-icon="book-open">Community Guide</a></p></td><td><a href="../../../.gitbook/assets/aws-card.png">aws-card.png</a></td><td><a href="https://aws.amazon.com/">https://aws.amazon.com/</a></td></tr></tbody></table>

***

### Relays only

These are relay only services.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><p><i class="fa-steam">:steam:</i> <strong>Steam</strong> relays are offered free when you launch your game on steam; there is a percentage of sales, as well initial fee when using Steam.</p><p>We have an official FishNet transport for Steam, and there are third-party Unity assets available as well to help utilize Steam Relays. A popular asset is <a href="https://github.com/heathen-engineering/Toolkit-for-Steamworks-Foundation">Toolkit for Steamworks Foundation by Heathen Engineering</a>.</p><ul><li>Free for games launched using Steam.</li><li>Session-based relays.</li><li>Match Making / Lobbies.</li><li>Friends list (Steam).</li></ul></td><td><a href="../../../.gitbook/assets/steam-card.png">steam-card.png</a></td><td><a href="https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay">https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay</a></td></tr><tr><td><p><strong>EOS</strong> provides a number of features for free. The API and documentation is a bit more complex, which may be difficult to beginners. A vetted third-party <a href="../../../fishnet-building-blocks/transports/fishyeos-epic-online-services.md">EOS transport</a> is available for FishNet, which may help with some of those hurdles.</p><p>A cross-platform friends list is possible with EOS but at the cost of complexity; eg: you will not be able to directly access your Steam friends via the EOS platform. With this in mind, match making with friends may be more difficult.</p><ul><li>Free to all developers.</li><li>Session-based relays.</li><li>*Match Making / Lobbies.</li><li>*Friends list.</li></ul></td><td><a href="../../../.gitbook/assets/eos-card.png">eos-card.png</a></td><td><a href="https://onlineservices.epicgames.com/en-US/services">https://onlineservices.epicgames.com/en-US/services</a></td></tr></tbody></table>

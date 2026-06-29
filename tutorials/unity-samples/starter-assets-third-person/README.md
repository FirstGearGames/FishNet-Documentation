---
description: >-
  These tutorials will show you how to convert the third person starter assets
  to support multiplayer.
---

# Starter Assets - Third Person

<figure><img src="https://assetstorev1-prd-cdn.unity3d.com/key-image/421988f6-a827-428b-86d4-9b815f15cdce.webp" alt=""><figcaption></figcaption></figure>

{% embed url="https://assetstore.unity.com/packages/essentials/starter-assets-thirdperson-updates-in-new-charactercontroller-pa-196526" %}

### Client-authoritative or server-authoritative?

We’ll demonstrate how to convert the asset using both methods. The **client-authoritative approach** is simpler to implement and offers players a smooth experience, but it carries the drawback of being more vulnerable to cheating. In contrast, the **server-authoritative approach** significantly reduces the risk of cheating, though it introduces higher input latency. This latency can be mitigated through **client-side prediction (CSP)**, which adds complexity but ensures responsiveness. The tutorial will guide you step by step through setting up CSP. However, before diving into the **server-authoritative + CSP tutorial**, it’s strongly recommended that you first review the [**Client-side Prediction guides**](../../../guides/features/prediction/what-is-client-side-prediction.md) to build a solid foundation.

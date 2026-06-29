---
description: >-
  FishyEOS integrates Epic Online Services with FishNet, enabling matchmaking
  and relay through the PlayEveryware EOS Plugin for Unity.
---

# FishyEOS (Epic Online Services)

## Description

EOS transport allows you to utilize Epic's Online Services, such as matchmaking and relay, as the base of your FishNet connection.\
\
FishyEOS uses the [PlayEveryware EOS Plugin for Unity SDK](https://eospluginforunity.playeveryware.com/), which is a wrapper API/SDK for developers using C# and Unity to take advantage of Epic's Online Services API.

***

## Compatibility

<table data-full-width="false" data-search="false"><thead><tr><th width="149">System</th><th width="198">Supported?</th><th>Epic Overlay Support?</th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Supported</mark></td><td><mark style="color:green;">Yes</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Supported</mark></td><td><mark style="color:red;">No</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Supported</mark></td><td><mark style="color:red;">No</mark></td></tr><tr><td>Web</td><td><mark style="color:red;">Not Supported</mark></td><td><mark style="color:red;">No</mark></td></tr><tr><td>IOS</td><td><mark style="color:green;">Supported</mark></td><td><mark style="color:red;">No</mark></td></tr><tr><td>Android</td><td><mark style="color:green;">Supported</mark></td><td><mark style="color:red;">No</mark></td></tr><tr><td>Xbox</td><td><mark style="color:yellow;">In Preview</mark></td><td><mark style="color:yellow;">Unknown</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:yellow;">In Preview</mark></td><td><mark style="color:yellow;">Unknown</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:yellow;">In Preview</mark></td><td><mark style="color:yellow;">Unknown</mark></td></tr></tbody></table>

***

## How to install

Please visit the GitHub locations for official installation and usage instructions.

Main: [https://github.com/ETdoFresh/FishyEOS](https://github.com/ETdoFresh/FishyEOS)

Fork: [https://github.com/FirstGearGames/FishyEOS](https://github.com/FirstGearGames/FishyEOS).

{% hint style="warning" %}
Remember that this transport requires a separate installation of the PlayEveryware EOS for Unity SDK. Installation instructions for that are included in the GitHub pages above.
{% endhint %}

***

## Component settings

* Maximum Clients: Maximum number of players which may be connected at once

#### EOS

* Socket Name: Socket ID \[Must be the same on all clients and server]
* Remote Server Product User ID: Server Product User ID. Must be set for remote clients.
* Auto Authenticate: Automatically Authenticate/Login to EOS Connect when starting server or client.
* Auto Connect Data: All the data necessary to login to EOS and connect to a remote peer.
  * Login Credential Type:
  * ID:
  * Token:
  * Display Name:
  * Automatically Create Device ID:
  * Auth Scope Flags:
  * Timeout:

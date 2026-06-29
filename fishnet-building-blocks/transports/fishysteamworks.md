---
description: >-
  FishySteamworks is a reformed version of FizzySteamworks, delivering major
  performance gains and full support for Heathen Engineering’s Steamworks SDK
  Toolkit.
---

# FishySteamworks (Steam)

## Description

FishySteamworks is FizzySteamworks reformed. FishySteamworks has significant performance improvements and has been made to fully support [Toolkit for Steamworks SDK](https://kb.heathen.group/toolkit-for-steamworks-sdk/steamworks) by _Heathen Engineering._

***

## Compatibility <a href="#server-and-host" id="server-and-host"></a>

<table data-full-width="false" data-search="false"><thead><tr><th width="149">System</th><th width="198">Supported?</th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Web</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>IOS</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>Android</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>Xbox</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:red;">Not Supported</mark></td></tr></tbody></table>

***

## How to install

Github: [https://github.com/FirstGearGames/FishySteamworks/](https://github.com/FirstGearGames/FishySteamworks/)\
\
Download and import the latest package from the [GitHub releases](https://github.com/FirstGearGames/FishySteamworks/releases) into your Unity FishNet project and add the **FishySteamworks** component to your [Transport Manager.](../components/managers/transportmanager/)

{% hint style="warning" %}
FishySteamworks is dependent upon [Steamworks.NET](https://steamworks.github.io/). An [installation article](https://steamworks.github.io/installation/#unity-instructions) for Unity is available on their webpage.
{% endhint %}

***

## Component settings <a href="#server-and-host" id="server-and-host"></a>

* **Steam App Id:** is the Id for your steam application. 480 is the default, which is often used as a test App Id.
* **Server Bind Address:** Which IP the server will bind/listen on. The default value is empty.
* **Port:** Indicates which port the server will listen on.
* **Peer To Peer:** Will allow clients to act as [host](../../guides/high-level-overview/terminology/server-client-host.md#server-and-host-2) when running your project. IP addresses are **NOT** revealed.
* **Maximum Clients:** is how many players can be connected to the server at once.
* **Client Address:** is the IP of the server, which clients connect to. Empty is the default value for local testing.

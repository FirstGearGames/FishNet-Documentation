---
description: >-
  The default transport of FishNet, utilizing the power, efficiency and
  reliability of LiteNetLib.
---

# Tugboat

## Description

Tugboat uses [LiteNetLib](https://github.com/RevenantX/LiteNetLib) to support reliable and unreliable messages. This is the default transport for Fish-Networking and will automatically be added to your [NetworkManager](../components/managers/network-manager.md) if no other [transport](./) has been added already. It supports a large variety of platforms, and is a good choice for many games.

***

## Compatibility <a href="#server-and-host" id="server-and-host"></a>

<table data-full-width="false" data-search="false"><thead><tr><th width="149">System</th><th width="198">Supported?</th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Web</td><td><mark style="color:red;">Not Supported</mark></td></tr><tr><td>IOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Android</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Xbox</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:green;">Fully Supported</mark></td></tr></tbody></table>

***

## How to install

Tugboat is the default transport of FishNet! Which means it comes with FishNet by default when Importing FishNet from your favorite source.

The [Transport Manager](../components/managers/transportmanager/) will automatically make this the default transport at runtime, if you did not add any other Transport to the Network Manager Game Object.

***

## Settings <a href="#server-and-host" id="server-and-host"></a>

<div align="left"><figure><img src="../../.gitbook/assets/tugboat-component.png" alt=""><figcaption><p>Default settings</p></figcaption></figure></div>

### :gear: Stop Sockets On Thread

> When enabled, causes the local server and client sockets to be stopped using a new thread. This option can be useful for ensuring that the socket shutdown process does not block the main thread, which may help with smoother shutdowns or prevent the application from freezing during network cleanup.

### :gear: Don't Route

> Enable **Don't Route** if you want your Tugboat server or client to send data directly to the local interface, avoiding any network routing by the operating system. This is an advanced option primarily for special networking requirements.

### :gear: **Unreliable MTU**

> This is the largest size an unreliable packet may be. When a single outbound data exceeds this value it is sent reliably. Smaller unreliable datas will be automatically split over multiple unreliable sends.

### :gear: **Reuse Address**

> Allows the same address and port to be used multiple times by the server. This can be useful if you wish to launch multiple builds or server instances on the same machine using the same configuration.

### :gear: I**Pv4 Bind Address**

> This is which address to bind the server to. If set and IPv4 is not available this will cause a socket error upon server start.

### :gear: **Enable IPv6**

> Enables IPv6 binding when available. In some cases you may want this disabled if you have an IPv6 interface you do not want used.

### :gear: **IPv6 Bind Address**

> This is which address to bind the server to. If set and IPv6 is not available this will cause a socket error upon server start.

### :gear: **Port**

> Is which port the server will listen on. This is also the port clients will connect to. In some instances you may need to change this at runtime using `TransportManager.SetPort()`.

### :gear: **Maximum Clients**

> **Maximum Clients** is the maximum active clients allowed before the transport begins to deny connections.

### :gear: **Client Address**

> This is which address to connect to as a client. This is typically your server address. Localhost is set as default for local testing.

---
description: Spawn an object over the network to represent each client's player.
---

# Preparing Your Player

For many games you will want to have a player object for each client. This guide will walk you through the process step by step.

{% hint style="success" %}
Before You Begin\
Ensure that you're **not in Play Mode** in the Unity Editor. Making changes during Play Mode will not persist after exiting.
{% endhint %}

{% stepper %}
{% step %}
### Create a player game object

In the Unity Scene Hierarchy, create a new 3D object of type **Capsule**.\
Select the capsule in the Hierarchy and rename it to **Player**.\
In the **Inspector**, click on **Add Component** and search for **NetworkObject**.\
Add the **NetworkObject** component to the capsule.

{% hint style="info" %}
The **NetworkObject** component is required for linking a game object over the network. It enables the object to synchronize its state across clients and the server.
{% endhint %}

<figure><img src="../../.gitbook/assets/add-networkobject-to-player.png" alt="NetworkObject component added to capsule"><figcaption><p>NetworkObject component added to capsule</p></figcaption></figure>
{% endstep %}

{% step %}
### Setup the player object for spawning

Drag the player game object into the project window to turn it into a prefab, then delete the game object from the scene.

<figure><img src="../../.gitbook/assets/create-player-prefab.png" alt="Turn the Player Object into a Prefab"><figcaption><p>Turn the player object into a prefab</p></figcaption></figure>

Select the NetworkManager in the scene and assign your new Player prefab into the **Player Prefab** field on the **PlayerSpawner** component.

<figure><img src="../../.gitbook/assets/assign-playerprefab-in-spawner.png" alt="Assign Player Prefab in Player Spawner"><figcaption><p>Assign player prefab in PlayerSpawner</p></figcaption></figure>
{% endstep %}

{% step %}
### Add spawn points

Create two empty Game Objects in the scene to act as spawn points. Position them where you'd like players to spawn on the map.\
Add these Game Objects as **Transform** references to the **PlayerSpawner** component in the **Spawns** list field.

{% hint style="info" %}
The **PlayerSpawner** uses these transforms to determine spawn locations, choosing them in order from top to bottom for each subsequent player and beginning again after reaching then final spawn point. If no spawn points are specified, the spawner defaults to using the prefab's transform properties for placement.
{% endhint %}

<figure><img src="../../.gitbook/assets/add-spawn-points.png" alt="Create and Assign Spawn Points"><figcaption><p>Create and assign spawn points</p></figcaption></figure>
{% endstep %}

{% step %}
### Test the player spawning

Save the scene and press the **Play** button in the Unity Editor.\
As soon as the server and client starts you should see the a capsule spawning as the player object at the location of the first spawn point.\
To see another player spawn you can build and run the game and which will then automatically connect as a client to the editor. You should then see a second player capsule spawn for the second player at the second spawn point.

<figure><img src="../../.gitbook/assets/build-connected-to-editor.png" alt="Built Game Connected to Editor"><figcaption><p>Built game connected to editor</p></figcaption></figure>

Alternatively, you can launch a second Unity Editor instance by using [Unity's Multiplayer Play Mode](https://docs-multiplayer.unity3d.com/mppm/current/about/) package or a third party package such as [ParrelSync](https://github.com/VeriorPies/ParrelSync?tab=readme-ov-file#parrelsync). Find out more about these options and how to use them with this tutorial: [testing-with-multiple-editors.md](../simple/testing-with-multiple-editors.md "mention").

<figure><img src="../../.gitbook/assets/mppm-connected-to-editor.png" alt="Multiplayer Play Mode Connected to Editor"><figcaption><p>Multiplayer Play Mode connected to editor</p></figcaption></figure>

{% hint style="info" %}
If a second game window shows _"Server failed to start"_ it just means multiple instances tried to use the same port. Only the first becomes the server; the rest run as clients, so the error is safe to ignore during local testing. If players don’t spawn correctly, check that **Error Pause** is disabled in the Unity console.
{% endhint %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Download the project files with these completed steps here, or explore the repository:

<a href="https://github.com/maxkratt/fish-networking-getting-started/releases/download/preparing-your-player-complete/preparing-your-player.unitypackage" class="button primary" data-icon="down-to-line">Source Files</a> <a href="https://github.com/maxkratt/fish-networking-getting-started/tree/preparing-your-player" class="button secondary" data-icon="github">Repository</a>
{% endhint %}

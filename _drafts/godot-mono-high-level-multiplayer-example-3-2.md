---
layout: post
title: Godot Mono High Level Multiplayer Example 3.2
date: 2020-02-02 04:00:00 +0000
summary: A brief example on how to use the High Level Multiplayer functionality in
  Godot 3.2
categories:
- Godot

---
Hello friends! I've started work on a multiplayer game using C# in Godot and noticed that the docs do a really good explanation on how to use the [High Level Multiplayer](https://docs.godotengine.org/en/3.2/tutorials/networking/high_level_multiplayer.html) functionality. The only issue is, the code examples and the demo project are currently only available in GDScript. I've decided to put together a quick demo Mono project, for those of you who are trying to find out how it works translated to C# You can find the code [here](https://github.com/ryynosaur/MonoHighLevelMultiplayer/tree/master). If you have any questions or suggestions reach out to me on [twitter](https://twitter.com/ryynosaur) or [where ever is easiest for you.](https://ryanforrester.ca/contact)

### What this demo will teach you

The demo contains examples of:

* How to host a game
* How to join a game
* How to leave a game
* synchronizing player movement across clients using the _puppet_ attribute 

#### Hosting

    var peer = new NetworkedMultiplayerENet();
    peer.CreateServer(default_port, 32);
    GetTree().NetworkPeer = peer;

#### Joining

    var clientPeer = new NetworkedMultiplayerENet();
    var result = clientPeer.CreateClient(address, default_port);
    GetTree().NetworkPeer = clientPeer;

#### Leaving

    ((NetworkedMultiplayerENet)GetTree().NetworkPeer).CloseConnection();
    GetTree().NetworkPeer = null;

##### On player connected

    private void PlayerConnected(int id)
    {
    	PlayerName = NameText.Text;
    
    	GD.Print($"tell other player my name is {PlayerName}");
    	// tell the player that just connected who we are by sending an rpc back to them with your name.
    	RpcId(id, nameof(RegisterPlayer), PlayerName);
    }

### Final Thoughts

Have fun on your multiplayer game making journey!

I may make a follow up to this at some point with setting up UPNP and ways to safely handle implementing it in Godot. 

Follow on [twitter](https://twitter.com/ryynosaur) or [other places](https://ryanforrester.ca/contact) for more cool Godot stuff! All the code for my demos can be found [here](https://github.com/ryynosaur).

Bye!
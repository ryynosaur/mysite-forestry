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

### Final Thoughts

You should now be on your way to making great moddable experiences!

One thing to mention about this project. When loading in the PNG files from the pck files Godot will through an error along the lines of "Failed to get modified time for: water.png". I did some digging and found that there was an [issue](https://github.com/godotengine/godot/issues/25318) already created for looking into this. From what I can tell for our use it is completely harmless.

Follow on [twitter](https://twitter.com/ryynosaur) or [other places](https://ryanforrester.ca/contact) for more cool Godot stuff! All the code for this project can be found [here](https://github.com/ryynosaur/GodotModExample).

Bye!
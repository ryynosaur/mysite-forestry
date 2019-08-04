---
layout: post
title: Opening a Web Browser is Really Easy in Godot
date: 2019-08-04 03:00:00 +0000
summary: Creating a button that when clicked goes to a web page can be done with this
  one line of code.
categories: []

---
Hello friends! This post is going to be short and sweet because well.. it only takes one line of code to open a web page up in Godot. The demo project can be found [here](https://github.com/ryynosaur/GodotOpenWebBrowser) with an example on how to create a button that when clicked will take you to my [twitter](https://twitter.com/ryynosaur).

## The magic line of code is..

    OS.ShellOpen("https://twitter.com/ryynosaur");

When **OS.ShellOpen** is passed a URL it will open up the default web browser of the operating system your game is running on. 

That's it! So now there is no excuse I shouldn't see a button to your Twitter, Discord, Facebook, Twitch, dog's Instagram from your home menu!

[Demo](https://github.com/ryynosaur/GodotOpenWebBrowser)
---
layout: Page
title: How to Add Mods to your Godot Game
date: 2019-08-01 03:00:00 +0000
summary: This tutorial will teach you how to add mods to your Godot game. We'll go
  over the steps on how to make a moddable Godot game as well as loading mods in at
  runtime.
categories:
- Godot

---
* hello and where to find the code
* what this project will teach
* Setting up the level project
* setting up the loader project

Hello friends! In this tutorial I will be going over the approach I used to implement mods into a Godot game I am creating. I have the demo project up [here](https://github.com/ryynosaur/GodotModExample) if you want to jump right into the code. I'm using the Mono version of Godot, but this code easily be reworked into GDScript with a bit of translating!

I recently started a project in Godot and one of the first requirements for it was I wanted to be able to add mod support. I was surprised to see that there wasn't a whole lot of resources around modding in the community, except for [a very brief summary in the Godot docs](https://docs.godotengine.org/en/3.1/getting_started/workflow/export/exporting_pcks.html) about how it could be achieved.

![](/uploads/2019/08/01/starfish400.png)

### What this tutorial will teach you

After this tutorial you will be able to make a fully moddable Godot game. Modders will be able to install Godot and work on a "Template" project that can be imported into your game. 
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
* Setting up the level project
* setting up the loader project

Hello friends! In this tutorial I will be going over the approach I used to implement mods into a Godot game I am creating. I have the demo project up [here](https://github.com/ryynosaur/GodotModExample) if you want to jump right into the code. I'm using the Mono version of Godot, but this code easily be reworked into GDScript with a bit of translating! If you have any questions or suggestions reach out to me on [twitter](https://twitter.com/ryynosaur) or [where ever is easiest for you.](https://ryanforrester.ca/contact)

I recently started a project in Godot and one of the first requirements for it was I wanted to be able to add mod support. I was surprised to see that there wasn't a whole lot of resources around modding in the community, except for [a very brief summary in the Godot docs](https://docs.godotengine.org/en/3.1/getting_started/workflow/export/exporting_pcks.html) about how it could be achieved.

![](/uploads/2019/08/01/starfish400.png)

### What this tutorial will teach you

After this tutorial you will be able to make a fully moddable Godot game. Modders will be able to install Godot and work on a "Template" project that can be imported into your game.

The **Mod Loader** project contain the code for loading in mods as well as our controllable player node.

The **Level Template** project contains the level our player will be moving around in. This is the project modders would use to create levels and load them into the game.

### Setting up the Mod Loader project

For this example, I've gone ahead and used Godot's quick movement tutorial to setup our crab buddy. [Here is the link you can follow for creating basic 2d movement.](https://docs.godotengine.org/en/3.1/tutorials/2d/2d_movement.html)

![](/uploads/2019/08/01/crab400.png)

Things start to get more interesting in the **Modloader.cs** file. I've attached this script to the base node:

    public class ModLoader : Node2D
    {
        [Export]
        public PackedScene Crab;
    
        public OptionButton DropDown;
    
        public override void _Ready()
        {
            DropDown = (OptionButton)GetNode("UI/Dropdown");
            DropDown.Connect("item_selected", this, "OnModSelected");
    
            // get an array of the folders in \Mods
            var modsList = System.IO.Directory.GetDirectories($"{System.IO.Directory.GetCurrentDirectory()}\\Mods");
    
            for(var i = 0; i < modsList.Length; i++)
            {
                // Add each mod to the dropdown list
                DropDown.AddItem(modsList[i].Split('\\').Last(), i);
            }
    
            LoadModLevel(DropDown.Items[0].ToString());
        }
    
        public void OnModSelected(int id)
        {
            var selectedMod = DropDown.GetItemText(DropDown.GetItemIndex(id));
            LoadModLevel(selectedMod);
        }
    
        private void LoadModLevel(string modName)
        {
            ProjectSettings.LoadResourcePack($"res://Mods/{modName}/LevelMod.pck");
    
            var importedScene = (PackedScene)ResourceLoader.Load($"res://{modName}.tscn");
    
            // remove all the nodes expect our dropdown
            foreach(Node child in GetChildren())
            {
                if(child.Name == "UI")
                {
                    continue;
                }
    
                child.QueueFree();
            }
    
            // load the level into the game
            AddChild(importedScene.Instance());
    
            // add mr crabs back
            AddChild(Crab.Instance());
        }
    }

That's all the the code I need to load a level into my game? It sure is! Let's go over the important bits by starting in the _Ready() method..

    var modsList = System.IO.Directory.GetDirectories($"{System.IO.Directory.GetCurrentDirectory()}\\Mods");

The above line of code will get a list of all the folders in mods folder. Mods will be in their own folder located in the Mods folder. The structure will look like this:

* root
  * Mods
    * Starfish Friends
      * All the mod files for Starfish Friends
    * No Friends
      * All the mod files for No Friends

The name of the mod folder is important not only because this example uses it to display the mod name in the game, but it also has to match the name of the scene in Level Template project. I'll explain why a little further down. 

### Setting up the Level Template project

There isn't much to go over in this project. This project contains our starfish friends as well as our sand and water assets! (the best programmer art I could make)
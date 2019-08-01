---
layout: post
title: How to Add Mods to your Godot Game
date: 2019-08-01 03:00:00 +0000
summary: This tutorial will teach you how to add mods to your Godot game. We'll go
  over the steps on how to make a moddable Godot game as well as loading mods in at
  runtime.
categories:
- Godot

---
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

Now let's take a look at this little bit of code in the LoadModLevel(string modName) method:

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

This is honestly the only thing required to load in our mod levels. We start by loading in the pck file that contains all our level resources in it. Next we get the PackedScene our level is in. Remember when I mentioned the mod folder name needs to match the scene name? It's because if we load up a project that has the same scene name as one that already exists it will overwrite the one that is already there! 

The rest of the code in that method simply removes the nodes that are currently present in the scene and adds in our new level scene.

### Setting up the Level Template project

This project contains our starfish friends as well as our sand and water assets! (The best programmer art)

Make sure you rename the scene for each level you make or else you will risk overwriting an existing scene.

![](/uploads/2019/08/01/starfishfriends.PNG)

Now let's build our level. go to **Project > Export**. We need to add a build template. At the top of the export page you should see **Add**. click it and then click on **Windows Desktop (Runnable)**. Change the **Export Path** to point to the Mods folder in the ModLoader project. The rest of the settings we can leave just the way they are.

Next let's click the **Export PCK/Zip** button at the bottom of the page. Our mod is now exported and ready to load!

![](/uploads/2019/08/01/export.PNG)

You should see the new mod in the drop down when you run the Mod Loader Project:

![](/uploads/2019/08/01/modslist.PNG)

### Final Thoughts

You should now be on your way to making great moddable experiences! 

One thing to mention about this project. When loading in the PNG files from the pck files Godot will through an error along the lines of "Failed to get modified time for: water.png". I did some digging and found that there was an [issue](https://github.com/godotengine/godot/issues/25318) already created for looking into this. From what I can tell for our use it is completely harmless.

Follow on [twitter](https://twitter.com/ryynosaur) or [other places](https://ryanforrester.ca/contact) for more cool Godot stuff! All the code for this project can be found [here](https://github.com/ryynosaur/GodotModExample).

Bye!
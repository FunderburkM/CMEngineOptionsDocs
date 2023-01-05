# CMEngineOptions - Docs

[Unreal Marketplace Profile](https://www.unrealengine.com/marketplace/en-US/profile/M+Funderburk).  
[Discord Server](https://discord.gg/QHTTMQ6Pqw).  

Documentation for the Universal Options Marketplace Plugin, V1.x! Available for Unreal Engine 4.26-27, 5.0 and 5.1.  

**These Docs are for Universal Options V1.x**.  
[For documentation on v2.0 version of this plugin, visit this branch.](https://github.com/FunderburkM/CMEngineOptionsDocs/tree/V2.0-Docs)  

***

Universal Engine Options is an optimized, modular, flexible, and easily implementable framework for general application settings. Control several profiles, save files, settings recovery, extendable settings, and input modes! Add exceptions to your rebindings, set your sound rules, and more!  

**Plug and Play** : Implemented at a ULocalPlayerSubsystem level, access and change Engine Options from any class, anywhere! For Multiplayer projects, this still ensures that only local machines are affected by said settings. Accessible to both C++ and Blueprints!  

**Extended Flexibility** : Implementation is up to the user. We do not tie functionality behind a UMG Class and/or limit the user to the settings and combinations that are set by default. Graphics, sounds, and input settings are largely customizable.  

**Modular Widget Framework** (1.1+) : Implementing your own widgets is now an incredibly simple process with the new framework.  

***  

## Table of Contents

[Index](/#Index)  
[Release Stats](/#Release-Stats)  
[Access Videos and Inside the Editor](/#Access)  
[Setup in the Engine](/#setup-in-engine)  
[Documentation Notes](/#Notes)  

## Index

- [Framework](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/Framework.md)  
Explains the base ways of the plugin, plugin settings, documentation, saving capabilities, and common practices  
- [Graphic Settings](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/Graphics.md)  
Covers the Graphic settings available, auto detect settings, among others.  
- [Audio Settings](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/Audio.md)  
Glosses over the audio settings available  
- [Input and Rebinding Settings](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/Input.md)  
Overviews the input rebinding system, manual and auto rebinding, key layouts, etc.  
- [Game Setting Settings](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/GameSettings.md)  
Explains how the Game Settings framework applies, and explains how to set it up  
- [Engine Subsystem Settings and Plugin Implementation](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md)  
  Covers the subsystem implementation, how to implement multiple systems and frameworks, using the saving system, plugin setup warnings, setting up FOV, etc  
- [Modular Widget setup introduced in 1.1](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/WidgetSetup.md)  
Explains the new widget framework and how to extend it yourself to fit your exact project needs!  

***

## Release Stats

Engine Versions:  

- 4.24 = 1.0
- 4.25 = 1.2.1  
- 4.26 = 1.2.5  
- 4.27 = 1.2.5  
- 5.00 = 1.2.5  

[Changelogs](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/ChangeLog.md)  

## Access

For marketplace elements, you will be able to access through “YourEngineInstallPath/Engine/Plugins/Marketplace/CM_Engine_Options”.  

[Documentation Video - 1.0](https://youtu.be/-e1E1KV_mTw)  
[Documentation Video - 1.1](https://youtu.be/ibudswpE9o0)  
[Test Build - 1.0](https://drive.google.com/file/d/16SRHBlQJdJcamcISwTUcFHT6MKPjcV56/view?usp=sharing)  

**Inside the Editor**  
To use the plugin, download the Plugin to your Engine and install it. Open your project, and head to Plugin. Search for CM_Engine_Options, enable, and restart your editor. To access the demo content, navigate to CM_Engine_Options Content.  
![image](https://user-images.githubusercontent.com/28312571/147303926-6881ab50-7c0b-4f32-8464-746842265b8f.png)

**BP Exposure Availability**
Please refer to CM_Engine_Options Content/BP_Options_Documentation.  

![image](https://user-images.githubusercontent.com/28312571/147325436-f71e257e-237a-4dce-acdf-d33de5c2e940.png)

## Setup in Engine

To use the plugin, you only need to enable the plugin inside your project. That will have the Subsystem set up!  

> This next part is only needed for 1.0  

In order to apply the currently loaded settings on Application launch as well as set the base functionality required to run things, we run Settings_Apply_Startup

![image](https://user-images.githubusercontent.com/28312571/147325730-063096f2-1a35-45d8-bb41-61f6e56c8a5d.png)

## NOTES

Check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md) for information on Rule sets, Blueprint Implementation, Implementing Field of View, System usage and Warnings.

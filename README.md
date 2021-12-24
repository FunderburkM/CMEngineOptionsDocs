# CMEngineOptionsDocs
Documentation for the Universal Options Marketplace Plugin / TTW Module   

[Support Email](mailto:mario.gtz.funderburk@gmail.com)  
[Discord](https://discord.gg/7ARv62rUum)  
[Unreal Marketplace Profile](https://www.unrealengine.com/marketplace/en-US/profile/M+Funderburk)  


Universal Engine Options is an optimized, modular, flexible, and easily implementable framework for general application settings. Control several profiles, save files, settings recovery, extendable settings, and input modes! Add exceptions to your rebindings, set your sound rules, and more!   

*Plug and Play* : Implemented at a ULocalPlayerSubsystem level, access and change Engine Options from any class, anywhere! For Multiplayer projects, this still ensures that only local machines are affected by said settings. Accessible to both C++ and Blueprints!  

*Extended Flexibility* : Implementation is up to the user. We do not tie functionality behind a UMG Class and/or limit the user to the settings and combinations that are set by default. Graphics, sounds, and input settings are largely customizable.    

*Modular Widget Framework [1.1+]* : Implementing your own widgets is now an incredibly simple process with the new framework.  

Engine Versions:  
- 4.24 = 1.0
- 4.25 = 1.1
- 4.26 = 1.1
- 4.27 = 1.1

**Plugin/Development Notes** 
Due to the size of the update in 1.1, its content will be thoroughly explained within the respective segments/functionality that got updated/revamped/introduced. For future updates, please refer to Changelogs for further information unless otherwise mentioned in this section. 

# Access  

For marketplace elements, you will be able to access through “YourEngineInstallPath/Engine/Plugins/Marketplace/CM_Engine_Options”.   
   
[Documentation Video - 1.0](https://youtu.be/-e1E1KV_mTw)  
[Documentation Video - 1.1](https://youtu.be/ibudswpE9o0)  
[Test Build - 1.0](https://drive.google.com/file/d/16SRHBlQJdJcamcISwTUcFHT6MKPjcV56/view?usp=sharing)   


## Inside the Editor
To use the plugin, download the Plugin to your Engine and install it. Open your project, and head to Plugin. Search for CM_Engine_Options, enable, and restart your editor. To access the demo content, navigate to CM_Engine_Options Content. 
 

# Framework

Universal Options is implemented through a [ULocalPlayerSubsystem](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Subsystems/ULocalPlayerSubsystem/index.html) , and is available to both C++ and blueprints. [Community Wiki](https://www.ue4community.wiki/programming-subsystems-29v70qij).  


## General

The system is very modular. From a Blueprint/Implementation perspective, the system walks by working with delegates; quickly and easily set up your classes/widgets to respond to settings’ events with ease. Delegates can be set to your UMG/HUD/Controller/etc classes easily!  




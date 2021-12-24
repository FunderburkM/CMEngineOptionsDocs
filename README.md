# CMEngineOptionsDocs
Documentation for the Universal Options Marketplace Plugin / TTW Module   

[Unreal Marketplace Profile](https://www.unrealengine.com/marketplace/en-US/profile/M+Funderburk) 

##### Table of Contents  
[Release](#Release-stats)  
[Access](#Access)  
[Framework](#Framework)



Universal Engine Options is an optimized, modular, flexible, and easily implementable framework for general application settings. Control several profiles, save files, settings recovery, extendable settings, and input modes! Add exceptions to your rebindings, set your sound rules, and more!   

**Plug and Play** : Implemented at a ULocalPlayerSubsystem level, access and change Engine Options from any class, anywhere! For Multiplayer projects, this still ensures that only local machines are affected by said settings. Accessible to both C++ and Blueprints!  

**Extended Flexibility** : Implementation is up to the user. We do not tie functionality behind a UMG Class and/or limit the user to the settings and combinations that are set by default. Graphics, sounds, and input settings are largely customizable.    

**Modular Widget Framework** (1.1+) : Implementing your own widgets is now an incredibly simple process with the new framework.  

## Release Stats   
Engine Versions:  
- 4.24 = 1.0
- 4.25 = 1.1
- 4.26 = 1.1
- 4.27 = 1.1

Release Dates
- 1.1 - December 20th 2021

# Access  

For marketplace elements, you will be able to access through “YourEngineInstallPath/Engine/Plugins/Marketplace/CM_Engine_Options”.   
   
[Documentation Video - 1.0](https://youtu.be/-e1E1KV_mTw)  
[Documentation Video - 1.1](https://youtu.be/ibudswpE9o0)  
[Test Build - 1.0](https://drive.google.com/file/d/16SRHBlQJdJcamcISwTUcFHT6MKPjcV56/view?usp=sharing)   


## Inside the Editor
To use the plugin, download the Plugin to your Engine and install it. Open your project, and head to Plugin. Search for CM_Engine_Options, enable, and restart your editor. To access the demo content, navigate to CM_Engine_Options Content. 
![image](https://user-images.githubusercontent.com/28312571/147303926-6881ab50-7c0b-4f32-8464-746842265b8f.png)
 

# Framework

Universal Options is implemented through a [ULocalPlayerSubsystem](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Subsystems/ULocalPlayerSubsystem/index.html) , and is available to both C++ and blueprints. [Community Wiki](https://www.ue4community.wiki/programming-subsystems-29v70qij).  
![image](https://user-images.githubusercontent.com/28312571/147303903-4ca4492a-7253-4084-ad01-1e0e1629f4ac.png)


## General

The system is very modular. From a Blueprint/Implementation perspective, the system walks by working with delegates; quickly and easily set up your classes/widgets to respond to settings’ events with ease. Delegates can be set to your UMG/HUD/Controller/etc classes easily!  

The system is greatly exposed, having over 50 functions exposed to blueprints.
![image](https://user-images.githubusercontent.com/28312571/147303881-813610d7-a0ca-4c14-a696-866448a62c8f.png)

The plugin also contains a large amount of commenting, and is decently self-docummented. 
![image](https://user-images.githubusercontent.com/28312571/147303849-bbfc0e4a-f217-447f-a9e3-c34491e8ea54.png)


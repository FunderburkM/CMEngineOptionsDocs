

# Framework

#### Table of Contents  
[General](#General)  
[Project Settings](#ProjectSettings)  
[Saving](#Save)  

Universal Options is implemented through a [ULocalPlayerSubsystem](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Subsystems/ULocalPlayerSubsystem/index.html) , and is available to both C++ and blueprints. [Community Wiki](https://www.ue4community.wiki/programming-subsystems-29v70qij).  
![image](https://user-images.githubusercontent.com/28312571/147303903-4ca4492a-7253-4084-ad01-1e0e1629f4ac.png)

***

# General

The system is very modular. From a Blueprint/Implementation perspective, the system walks by working with delegates; quickly and easily set up your classes/widgets to respond to settings’ events with ease. Delegates can be set to your UMG/HUD/Controller/etc classes easily!  

The system is greatly exposed, having over 50 functions exposed to blueprints.
![image](https://user-images.githubusercontent.com/28312571/147303881-813610d7-a0ca-4c14-a696-866448a62c8f.png)

The plugin also contains a large amount of commenting, and is decently self-docummented. 
![image](https://user-images.githubusercontent.com/28312571/147303849-bbfc0e4a-f217-447f-a9e3-c34491e8ea54.png)

***  

# Project Settings 
In Project Settings -> Plugins -> CM_Engine_Options

![image](https://user-images.githubusercontent.com/28312571/147304793-d9a5248b-364a-448c-b4bc-7ce91d2c4bcb.png)   

These are the default Settings. Every time you Reset a profile/subprofile, these are the settings that will get set to.
Furthermore, if a profile does not currently have any flags saved at the savegame level, it is here where settings are grabbed on by default. Check Save Profile below for more information.  

**Global Settings**   
Controls whether the respective modules are enabled. Disabling any of these will control whether the related functionality actually gets applied or not.   
**Graphic Settings**  
Controls User Settings values relative to graphics: resolution, vsync, dynamic res, graphical fidelity, field of view, auto detect (1.1+), among others.  
**Input Settings**  
Controls Input profile switching, input rebinding options and capabilities.   
**Sound Settings**  
Controls Settings to be applied at the audio controller level, such as sound Mix and related sound classes with their respective custom values.  
**Game Settings**  
While the settings have no functionality by themselves, this module provides a flexible framework to create, store, and modify an unlimited amount of values at runtime.  

***

# Save 

For Graphics, Sounds, Input, and Game (All)  
![image](https://user-images.githubusercontent.com/28312571/147305211-02fb1033-4d30-4f57-8bc1-13828dd2fd7b.png)

- Saved Settings have a priority over the Settings Panel. If you have saved a config and wish to test config settings, delete the save file in Project/Saved/SaveGames.  
- You can save, Clear, and Reset specific elements. The major profiles are: Graphics, Input, Sound, and Game Settings; the latter two have sub profiles that can be affected independently.  
- Elements can be Saved, applied, Check for Saves, Reset to Config Defaults, and Save Cleared both Independently and as groups.  
- The system interacts in the following way: By default, Config File [Project Settings] are loaded on application startup. In the same context where defaults are loaded, we check/create the save game file and check if any profiles/subprofiles have the “Saved” flag for them. If we find them, those will update the current settings.   
- On Check Pending Save, we compare against the currently saved values; not the default ones!   

### Save Notes

Whenever you make large changes to your settings, including:  
* Renaming/Removing existing profiles:  
  * Sound Profiles  
  * Game Settings  
  * Input Profiles  
  * Profiles inside the Graphic Settings Data Table  

We recommend deleting the previous save file. Depending on your configuration settings (and/or especially if you're on 1.0), trying to load those values may result in a crash.  

> You can check specific notes about how to deal with these settings in each [Page](README.md#Index).  


### 1.1 

#### Rename your Sav file name! 

You can change the name of the save file from cpp.  
> You can do this if you either have the plugin inside YourProject/Plugins/ or if you're working with a source engine.  


The Save slot name is defined in Source/CM_Engine_Options/Private/CS_Engine_Options.cpp.  You can change this value and recompile. For cleanliness purposes, we recommend removing the previous save file name.  

 ```cpp
 const FString FEngineOptionsStaticNames::STRING_SaveSlotName = FString(TEXT("SG_Engine_Options"));
 ```


#### Load Save Config

![image](https://user-images.githubusercontent.com/28312571/147305307-da06c0ce-1c09-4ed4-b9e0-90ce2608beec.png)  

Saved Profiles/Subprofiles may take precedence over custom default C++ values/Plugin Project Settings/Graphics Data Table depending on your Load Save Settings. Keep Saved will override changes in default values. For Graphic Options, also check the value of bKeepSavedValuesWhenCheckingConfig, as it will influence how the data table options are treated for non custom graphic profiles.  

Load Save Config can be found in Graphics, Sound, and Game Settings modules.   
- Keep Config: Will only keep/have the layers/profiles that are found in the default configuration. If the saved elements do not contain an entry/profile. Add it. If they contain one that the configuration does not have, delete it.  
- Ensure Config: Will add any entries/layers that the save file is missing.  
- Keep Save: Will maintain save files as absolute options  





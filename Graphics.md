# Graphics 

#### Table of Contents
[Implementation](#implementation)  
[Default Settings](#default-settings)   
[Auto Detect Settings](#autodetect)  
[Localization and Custom Text Setups](#updatedtext)  
[Technical Info](#technical)  

For Information on Graphic Settings related widgets (1.1+), please [check this page](WidgetSetup.md#graphics-widget).  
For Information on saving and loading configuration, please check our [saving system](Framework.md#save).  
For Information on the Redirect System, please check our [Redirect page](Framework.md#redirects).  

For Graphic Quality Settings, you can have as many Graphic Profiles as you wish; edit the datatable/make your own and set as many standard/custom profiles as you want to!
Check Graphics Profile (DataTable) for more information.  
You also have access to the Field of View Element via the Camera Interface. Check Technical for more details.   

![image](https://user-images.githubusercontent.com/28312571/147317134-534d1dfd-a0b3-46b3-840d-cf6bdbea3587.png)

> 1.1 Update - Additionally to the widget revamps, Added support for Gamma, and more FoV interface options: now allows controller, playercameramanager, and manager view target.

# Implementation

To set up your Settings, go to Project Settings -> Plugins Section -> CM_Engine_Options and scroll to the Graphic Settings Section.  

![image](https://user-images.githubusercontent.com/28312571/158044214-6a6c7bd9-0198-4a16-b823-3cf97313fd8b.png)

Let's break it down a bit:  

- `GraphicTypeNameSettings` and `GraphicQualityNameSettings` used by Widgets, these give you localizable versions of Enumerator Values. Check [Updates](updatedtext) `1.1+`  
- `bAutoLoadAutoDetectOnProfile` On our first run and/or when there are no saved settings for graphics at the save level, whether we should automatically auto detect. Check [Updates](#autodetect) `1.1+`  
- AutoDetectOnProfileDefaultProfileName` whenever running Auto Detect (either on startup or by using AutoDetect Default), what profile to put this information in. Check [Updates](#autodetect) `1.1+`  
- `AutoDetectSettings`  The Default Values for running and applying the benchmark. Check [Updates](#autodetect) `1.1+`  
- `GraphicsProfile` The managing class for the graphic parameters.  

## Graphics Profile  

Let's first break down things up. Not Ever variable in Graphics Profile is exposed to default. We'll start by explaining these:  

### Default Settings

- `DataTable` Holds the pointer to the data table that we load our base values from.  
- `ProfileCurrent` Holds the current active profile within the base values. For Default Settings, this is the profile that we get loaded to when Auto Detect is turned off.  
- `DefaultGraphicsLoadResolution` When there is no saved graphics configuration, what resolution do we want to launch in. `1.1+`  
- `DefaultGraphicsLoadWindow` When there is no saved graphics configuration, what mode do we want to launch in.  `1.1+`  
- `ForcedResolution` when Load Resolution == Force Value, this is the value that we use to load into it. `1.1+`  
- `FieldofView` the field of view that we'll use to communicate with our Interface. Check the [Subsystem](EngineOptionsSubsystem.md#field-of-view).  
- `ResolutionScale` the scale at which we render our Resolution. In Percentage. 100 == full resolution.  
- `Gamma` the gamma value that is used. Gamma 0 = no gamma applied.  
- `bVSyncIsOn` whether VSync is enabled.  
- `bIgnoreRefreshRate` whenever fetching resolutions, whether we should ignore multiple entries at the same resolution that only differ in refresh rate.  
- `bLogResults` Whether we showcase most of our result keys on the Output Log.   

### Other Settings

- `Layers` This holds the collection of values. Each layer represents one fidelity configuration.  
- `SavedResolution` stores the value of our current resolution.  
- `UsableResolutions` stores our possibly used resolutions.  
- `WindowMode` what is our current applied Window Mode  
- `FramerateLimit` stores the array of accepted framerate limits.  

### Data Table System

Your Graphic Profiles for settings is controlled by a Data Table. Here, you can set how many profiles you want, and what settings you want your profiles to have.  
You can also have Custom Profiles, giving the user the ability to customize their settings.  

Let's break this a little bit:  

- `MapGraphicsType` holds the configuration of different quality settings type and their respective value assigned to them.  
- `Profile` Holds the configuration of the profile itself. **We do not use the RowName at the Data Table for the name of the profile; it was to be set here.**  
  - `Entry` Holds the name of the Profile  
  - `KeyValue` holds how it follows the rules:  
    - 0 is Standard, meaning that we can't cycle individual settings within this profile at runtime
    - 1 is Custom, meaning that we can cycle their individual settings but we still depend on `Settings` Variable (check below)  
    - 2 is Custom Override. Like Custom, but we're not limited by `Settings`  
- `Settings` holds the Array of setting ranges that are not allowed as part of the cycle at individual levels (custom profiles).  Say for instance that in your Medium Configuration profile _should not_ allow the user to be able to set to Post Processing to Low. You can add this entry in Settings.  

Settings in Graphics Profile           |  Showcasing the Data Table
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/28312571/147317515-b05419cd-d428-4515-ad37-d25916ccd667.png)  |  ![image](https://user-images.githubusercontent.com/28312571/147317543-1020b389-baba-41d4-b9fe-fbdb0287fbb9.png)  

![image](https://user-images.githubusercontent.com/28312571/147317580-a1237872-3735-4ccc-a80d-29725289dc44.png)
> Showcasing how custom profiles behave differently from standard (non-custom) ones.   

## Notes 

* Whenever renaming/removing Profile names at the data table level and/or changing data tables, make sure that you do fill Profile information. This meaning that the Data Table Row Name and the Graphics Basic Profile Entry name have appropriate names.  
* Make sure that the entries in your default plugin settings graphics options have names that match the information that you're after. For example if you no longer have a Custom or an Epic named profile, make sure to update the Graphics Panel. **Not Doing so may result in a crash.** In a development environment we also recommend clearing your save file at the graphics level and/or removing the .sav file in YourProject/Saved/SaveGames for best results.  

## Listening To Events

We can listen to overall major events from the subsystem such as the following:  
![image](https://user-images.githubusercontent.com/28312571/158044903-de311563-f6e6-4d9a-ae5c-73f2f3fd7007.png)  

For more information on Functions, please check [Technical](#technical).

# Updates

## Default Settings

> This is available on Update 1.1  

Default Window: You can now set default window settings, allowing you control over the user’s first/no save launch. You can specify type of default resolution and window mode.

![image](https://user-images.githubusercontent.com/28312571/147317874-4c3b641f-bd6a-4d49-a211-f5067c3b4653.png)


# AutoDetect

> This is available on Update 1.1  

Auto Detect is here! Set your auto detect criteria, as well as whether to apply and save it on the first run :)  

Auto Detect Settings:  
* Work Scale will determine the amount of work to be done by the Benchmarking system. 
* The CPU and GPU multipliers will modify the results of the benchmark.  

We will use the result of the benchmark to set the settings values (Economy-Low-Medium-High-Epic-Cinematic) based on `bSetSettings` and will decide to save its results based on `bSave`. We will set the results on `AutoDetectOnProfileDefaultProfileName`. **Make sure that this value exists in your profiles!**  

![image](https://user-images.githubusercontent.com/28312571/147317935-ab85e485-3bdd-48d4-bf12-a31fbc195bd3.png)

Easily switch to it with a single function :D   

![image](https://user-images.githubusercontent.com/28312571/147317974-bd53f968-3e13-4562-ad18-55b3648093cd.png)

# UpdatedText

> This is available on Update 1.1    


Used by both the 1.0 and 1.1 Widgets, you can now set your custom text [localizable] to display!

![image](https://user-images.githubusercontent.com/28312571/147318084-c1d22428-e4e9-4aa7-a41d-839c76914f84.png)


# Technical

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md).  

The functions/elements you will likely be referencing/using are the following (Images taken from `/Content/BP_Options_Documentation.uasset`)  

![image](https://user-images.githubusercontent.com/28312571/158044919-e9b0b830-661b-4c07-a0c6-5f619b9026a6.png)

![image](https://user-images.githubusercontent.com/28312571/158044943-2cabc78d-483f-418f-84d9-a1d1152944fe.png)  


Generally explained, it is Implemented through the classes:
- FEngineOptionsProfileName   
  Handles the basic information for Profile Information, their name and whether or not they’re Standard/Custom/Custom without Rules.   
  - Custom vs Custom w/o Rules are different since for Quality Cycling, Custom profiles will check the Settings array (Defined at the FEngineOptionGraphicsSettings.BlockedSettings). This provides further control over what the user can and cannot set. 
- FEngineOptionsGraphicsResolution   
  Is the blueprint-exposed version of FScreenResolutionRHI. We use that structure to handle resolutions.
- FEngineOptionsGraphicsBasic   
  Is the foundation for a Graphic Profile. Using FEngineOptionProfileName as an identifier, it stores a Map for the Graphics Type settings and their respective Quality Setting. 
- FEngineOptionsGraphicsSettings   
  As mentioned above, provides a profile to block settings. In practice, only Custom Profiles reference this structure. 
- FEngineOptionsGraphicsLayer   
  Compatible with DataTables. This is the Wrapped information for a single “graphic profile.” Handles the Swapping and storing of information.
- FEngineOptionsGraphicsProfile   
  Is the implementing class for all graphics in this plugin. Stores the profile (identified via Name), FoV (implemented to Controller/Pawn via Interface), ResolutionScale, VSync, Resolution,  Window Mode, etc. 
- FEngineOptionsGraphicsAutoDetectSettings (1.1+)   
  Handles the calculations for Auto Detecting operations.

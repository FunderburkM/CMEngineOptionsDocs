# Graphics 

#### Table of Contents
[Graphics Profile](#Graphics-Profile)  
[Default Settings](#DefaultSettings)   
[Auto Detect Settings](#AutoDetect)  
[Localization and Custom Text Setups](#UpdateText)  
[Technical Info](#Technical)  

For Information on Graphic Settings related widgets (1.1+), please [check this page](WidgetSetup.md#graphics-widget).  
For Information on saving and loading configuration, please check our [saving system](Framework.md#save).  

For Graphic Quality Settings, you can have as many Graphic Profiles as you wish; edit the datatable/make your own and set as many standard/custom profiles as you want to!
Check Graphics Profile (DataTable) for more information.  
You also have access to the Field of View Element via the Camera Interface. Check Technical for more details.   

![image](https://user-images.githubusercontent.com/28312571/147317134-534d1dfd-a0b3-46b3-840d-cf6bdbea3587.png)

> 1.1 Update - Additionally to the widget revamps, Added support for Gamma, and more FoV interface options: now allows controller, playercameramanager, and manager view target.


# Graphics Profile  

Your Graphic Profiles for settings is controlled by a Data Table. Here, you can set how many profiles you want, and what settings you want your profiles to have.  
You can also have Custom Profiles, giving the user the ability to customize their settings. 

Settings in Graphics Profile           |  Showcasing the Data Table
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/28312571/147317515-b05419cd-d428-4515-ad37-d25916ccd667.png)  |  ![image](https://user-images.githubusercontent.com/28312571/147317543-1020b389-baba-41d4-b9fe-fbdb0287fbb9.png) 
  

![image](https://user-images.githubusercontent.com/28312571/147317580-a1237872-3735-4ccc-a80d-29725289dc44.png)
> Showcasing how custom profiles behave differently from standard (non-custom) ones.   

## Notes 

* Whenever renaming/removing Profile names at the data table level and/or changing data tables, make sure that you do fill Profile information. This meaning that the Data Table Row Name and the Graphics Basic Profile Entry name have appropriate names.  
* Make sure that the entries in your default plugin settings graphics options have names that match the information that you're after. For example if you no longer have a Custom or an Epic named profile, make sure to update the Graphics Panel. **Not Doing so may result in a crash.** In a development environment we also recommend clearing your save file at the graphics level and/or removing the .sav file in YourProject/Saved/SaveGames for best results.  

## Default Settings

> This is available on Update 1.1  

Default Window: You can now set default window settings, allowing you control over the user’s first/no save launch. You can specify type of default resolution and window mode.

![image](https://user-images.githubusercontent.com/28312571/147317874-4c3b641f-bd6a-4d49-a211-f5067c3b4653.png)


# AutoDetect

> This is available on Update 1.1  

Auto Detect is here! Set your auto detect criteria, as well as whether to apply and save it on the first run :)  

```text
* Work Scale will determine the amount of work to be done by the Benchmarking system. 
* The CPU and GPU multipliers will modify the results of the benchmark.  

We will used the result of the benchmark to set the settings values (Economy-Low-Medium-High-Epic-Cinematic).  
```

![image](https://user-images.githubusercontent.com/28312571/147317935-ab85e485-3bdd-48d4-bf12-a31fbc195bd3.png)

Easily switch to it with a single function :D   

![image](https://user-images.githubusercontent.com/28312571/147317974-bd53f968-3e13-4562-ad18-55b3648093cd.png)

# UpdatedText

> This is available on Update 1.1    


Used by both the 1.0 and 1.1 Widgets, you can now set your custom text [localizable] to display!

![image](https://user-images.githubusercontent.com/28312571/147318084-c1d22428-e4e9-4aa7-a41d-839c76914f84.png)


# Technical

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md).  

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

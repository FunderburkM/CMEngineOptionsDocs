# Game Settings

#### Table of Contents
[Broadcasting Method](#Broadcasting-method)  
[Warnings](#Warnings)  
[Updates](#Updates)  
[Technical](#Technical)  

For Information on Game Settings related widgets (1.1+), please [check this page](WidgetSetup.md#game-settings-widget).  
For Information on saving and loading configuration, please check our [saving system](Framework.md#save).

## Setting Types

We offer three types of settings:  
* Float Settings:  
    Best used with numeric types of settings that may involve decimals, negative numbers, or numbers larger than 255.    
* String Settings:  
    Best used with non numeric values. Any kind of string works here.  
* Byte Settings:  
    Best used with values lesser than 255. Is/can also be used for Enumerator conversions, booleans (0 false 1 true).  
    
> We strongly recommend using Byte settings to account for boolean settings. You can check example usecases in the V1 example Widgets.  

## Overview

Easy and powerful way to store endless settings of different types! Store as many settings as you want in as many categories as you decide!  

![image](https://user-images.githubusercontent.com/28312571/147318780-1ceb3784-802e-42e5-827f-4d1c0e416794.png)

Furthermore, you can have saved Values. While in this pack they have no functionality implemented to them, you can set multiple options and systems. Implementation at an external level is also very simple. Check Game Settings [Expandable]  

![image](https://user-images.githubusercontent.com/28312571/147318864-7e2cb7b5-3cfa-4139-b097-0f1070207f9b.png)  
> Example of a widget using Game Settings of the profile Gameplay  

You can have as many Groups as you want. Furthermore, each group holds Maps on Byte, Float, and String Values.  You can Save / De save them per Group, per Type, and easily access them everywhere. Furthermore [C++ Only], you can add/Remove Groups and Settings during Runtime!   

When setting Values, you can decide to either broadcast the set value or add it to pending broadcasts. By default, we don’t broadcast on Sent. If you decide to not broadcast the value on edited however, you MUST run the Apply Pending Save function for that profile [if not All profiles] so that the Modified Delegates do in fact run.    

## Broadcast Method  
When Setting a Game Setting Option, you have the Option to Broadcast it. If Broadcast is true, it will broadcast that setting change at the time of setting the value. Otherwise, it will broadcast it at the time of saving.  

## Warnings

Do not add profiles on RUNTIME! However, you can add Settings (Bytes/Floats/Strings) by modifying that profile’s Structure. Example: 
```cpp
LPSS_Engine_Options->GameSettings.Layers[ProfileIndex].Bytes.Add(“MySettingName”, 2));
```

# Updates
> This is available in 1.1     


In 1.0, fetching multiple settings at once was a painful operation. In 1.1, You can now fetch profiles by Map Values [which will return the names and settings, and by names].
Check BP_Documentation for more details.

## Game Settings Data Asset

> This is available in 1.2  

You can set the data asset at Default settings and can also set on Runtime! this eases changing configurations as well as saving your needed presets for different needs! The structure follows the same principle as the Settings at the Plugin Level.  

![image](https://user-images.githubusercontent.com/28312571/158042275-a11150cd-cf6e-4136-8f5c-6650f77edc68.png)  

You can change the Data Asset Configuration at runtime using this function:  
![image](https://user-images.githubusercontent.com/28312571/158042297-58afd101-fc20-43f7-901e-ec96a9a24ae2.png)  



## Technical 

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md). This also covers how to hook up listeners to actually apply your changes in your systems!  

The functions/elements you will likely be referencing/using are the following (Image taken from `/Content/BP_Options_Documentation.uasset`)  
![image](https://user-images.githubusercontent.com/28312571/158042325-85c20bc5-c4f3-4579-b601-d29a7d7e3b56.png)


- FEngineOptionGameLayer stores the Game Settings Maps: Byte Map, Float Map, and String Map. Each can be saved individually.
- FEngineOptionGameProfile stores an Array of Layers, giving the control of multiple Map Settings through multiple menus / etc.

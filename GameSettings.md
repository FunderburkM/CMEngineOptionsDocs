# Game Settings

#### Table of Contents
[Broadcasting Method](#Broadcasting-method)  
[Warnings](#Warnings)  
[Updates](#Updates)  
[Technical](#Technical)  

Easy and powerful way to store endless settings of different types! Store as many settings as you want in as many categories as you decide!  

![image](https://user-images.githubusercontent.com/28312571/147318780-1ceb3784-802e-42e5-827f-4d1c0e416794.png)

Furthermore, you can have saved Values. While in this pack they have no functionality implemented to them, you can set multiple options and systems. Implementation at an external level is also very simple. Check Game Settings [Expandable]  

![image](https://user-images.githubusercontent.com/28312571/147318864-7e2cb7b5-3cfa-4139-b097-0f1070207f9b.png)  
> Example of a widget using Game Settings of the profile Gameplay  

You can have as many Groups as you want. Furthermore, each group holds Maps on Byte, Float, and String Values.  You can Save / De save them per Group, per Type, and easily access them everywhere. Furthermore [C++ Only], you can add/Remove Groups and Settings during Runtime!   

When setting Values, you can decide to either broadcast the set value or add it to pending broadcasts. By default, we don’t broadcast on Sent. If you decide to not broadcast the value on edited however, you MUST run the Apply Pending Save function for that profile [if not All profiles] so that the Modified Delegates do in fact run.    

# Broadcast Method  
When Setting a Game Setting Option, you have the Option to Broadcast it. If Broadcast is true, it will broadcast that setting change at the time of setting the value. Otherwise, it will broadcast it at the time of saving.  

# Warnings

Do not add profiles on RUNTIME! However, you can add Settings (Bytes/Floats/Strings) by modifying that profile’s Structure (LPSS_Engine_Options->GameSettings.Layers[ProfileIndex].Bytes.Add(“MySettingName”, 2))

# Updates
> This is available in 1.1     


In 1.0, fetching multiple settings at once was a painful operation. In 1.1, You can now fetch profiles by Map Values [which will return the names and settings, and by names].
Check BP_Documentation for more details.


# Technical 

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md).  

- FEngineOptionGameLayer stores the Game Settings Maps: Byte Map, Float Map, and String Map. Each can be saved individually.
- FEngineOptionGameProfile stores an Array of Layers, giving the control of multiple Map Settings through multiple menus / etc.

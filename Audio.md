# Audio


For Information on Audio Settings related widgets (1.1+), please [check this page](WidgetSetup.md#audio-widget).  
For Information on saving and loading configuration, please check our [saving system](Framework.md#save).


# Implementation  

Set your own mix, and use your own classes! Furthermore, you can set as many as you may need! You can also set volume/pitch/fade in time individually.   

To set up your Audio Settings, go to `Project Settings -> Plugins Section -> CM_Engine_Options` and scroll to the `Audio` Section. You can set your Projectiles and Sound Classes that you want to be able to modify, save, and set at runtime.  Each Layer must have a unique name; we use this name to identify the configuration throughout our systems; this includes widgets.  

Sound Settings in Sound Profile           |    Sample procedurally generated Sound widgets in 1.1
:-------------------------:|:----------------------------------------------------------:
![](https://user-images.githubusercontent.com/28312571/147318230-3d8ba747-8b68-4d2f-95c7-20d179fc83a3.png) |  ![](https://user-images.githubusercontent.com/28312571/147318314-324fdcc6-dce0-4496-8ffb-f33bbb727c00.png)  

# Updates

## Sound Settings Data Asset

> This feature is available on 1.2  

You can set the data asset at Default settings and can also set on Runtime! this eases changing configurations as well as saving your needed presets for different needs! The structure follows the same principle as the Settings at the Plugin Level.  

![image](https://user-images.githubusercontent.com/28312571/158042064-b1b7abff-7112-40ed-90f0-57c3acdbb983.png)  

> **Notes**: if the Data asset has a valid Sound Mix Class set, we will try to set this to be the current one applied. Use this with caution.  

You can change the Data Asset Configuration at runtime using this function:  
![image](https://user-images.githubusercontent.com/28312571/158042136-f10a1af6-3e03-4772-b118-8d2c9e7b6c89.png)  


# Technical

For more information on how it's used, check [Engine Options Subsystem](EngineOptionsSubsystem.md).  

The functions/elements you will likely be referencing/using are the following (Image taken from `/Content/BP_Options_Documentation.uasset`)  

![image](https://user-images.githubusercontent.com/28312571/158042158-7d0b0f6d-959e-4b2f-ab42-6cccd65a263b.png)


- FEngineOptionSoundLayer  Acts as a “sound Class” representative. Stores name as ID, float and Sound Class Values. It manages Volume, Pitch, and Fade Distance
- FEngineOptionSoundProfile implements the multiple Sound Layers and a Sound Mix

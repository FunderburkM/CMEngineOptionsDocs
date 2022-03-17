# Modular Widget Setup (1.1+)  

As of 1.1, we do not recommend using the Widgets inside CM_Engine_Options Content/Test. Nevertheless, for reference and legacy purposes, they will remain in the plugin. This document will only cover widget setup and workflow with the widget setup implemented on 1.1+. 

#### Table of contents   
[Hierarchy, Modularity, and C++ magic](#hierarchy)  
- [Concepts](#concepts)  
- [General Convention](#general-convention)  
- [Class hierarchy](#class-hierarchy)     

[Blueprint Setup of Widgets](#blueprint-setup)
- [General Widget](#general-widget)  

[Widget Example Framework](#examples) (1.2+)  
> Please make sure you read through at least Hierarchy and some general information of Blueprint Setup of Widgets in their individual widget pages to have a general understanding of the framework. Base Important information for Example and Generic/Template widgets in C++ and BP will be mentioned there instead of in the [Examples](#examples) section. In there we will just cover how we've implemented these specifically.     

# Hierarchy
The first section of this segment will overview the C++ classes and general conventions. Once that is covered, we will go over the 1.1 implementations and steps needed to roll out your own setups! We’ll be including the Technical Elements for C++ widgets in this part. The widgets can be found inside CM_EngineOptions/public|private/Widgets.

## Concepts
Generally speaking, these widgets mirror/replicate the setups from the widgets in 1.0, in a polished and much more modular/extensible way. Their minimal goal is to accomplish everything [and a bit more] than the 1.0 widgets accomplish as well as provide an easy and extendable framework for custom implementation. 

## General Convention

- Widget Names start with CM_EngineOptions_WidgetName_Type..We treat WidgetName to be the specific category it represents. Example: CM_EngineOptions_Graphics_Slot.
- WidgetName_Slot is treated for Rows, such as input entries, graphic quality elements, etc. Each Slot exists for its own category
- WidgetName_Page is the container for different slots of that category or general pages within its own category.
- Widget_General_Page being the exception as this widget contains the different pages and such
- WidgetName_PageCollection [Game Settings only] contains multiple pages and aids in switching between page subprofiles
- Each widget makes itself responsible for sending and receiving values accordingly and [for the most part -check exceptions in the BP setup] on its own.

## Class hierarchy

- Graphics
  - **Graphic_Slot**  
  Used for Quality Slot entries, such as Textures/Shadows/etc. Each entry is responsible for/modifies a specific quality setting. 
  - **Graphic_SlotProfile**  
  Used for Quality Profile switching which affects all quality types -and therefore is treated as the “global” switcher. Switches in between the different layers that Graphic Profile has [Low/Medium/Custom] etc
  - **Graphic_Page** 
  Auto generates Graphic Slot and Profile, and serves as the baseline for storing Graphic-related widgets. Check the Blueprint setup for info. Resolution/Frame Limit/Vsync/among others are added at the BP level.
  
**You can check the Graphics Widget page [here](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/WidgetSetupGraphics.md)**.  

- Audio
  - **Audio_Slot**  
  Used to represent a specific entry of a given Sound Layer. You can have Sound::Volume, Sound::Pitch, and Sound::FadeDuration
  - **Audio_Page**  
  Auto generates/detects Audio Slots, and can generate as many widgets as there are settings for it. Check the Blueprint setup for info. If auto generation is turned off, it can auto detect Audio Slot classes added at design time.

**You can check the Audio Widget page [here](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/WidgetSetupAudio.md)**.  

- Input
  - **Input_Slot**  
  Represents a given binding entry; can be either Action or Axis Binding. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture).
  - **Input_OptionsWindow**  
  Is the widget that pops up whenever a binding request is made, either by listening key or manual binding. Is summoned by Input_Slot by the user’s request.
  - **Input_KeyEntry**  
  Is what OptionsWindow uses to fill its dropdown. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture).
  - **Input_Page**  
  Handles the different profile switching, generation, and Input Request handling. Input Profiles present get generated at the BP level. 


**You can check the Input Widget page [here](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/WidgetSetupInput.md)**.  

- Game Settings  

  - **Game_Slot** (Available in 1.2)  
  Represents an individual Game Setting. Check [Examples](#examples) for more information.  
  - **Game_Page**  
  Represents an individual Game Profile [for example in the default project, the Gameplay Profile that contains Perma Death and Mouse Multiplier settings]. The Individual Slots are added at the BP level.
  
  > Game Pages also have an additional filtering concept for Subsystem Updates. Please check [the widget section](#game-settings-widget).  
  
  - **Game_PageCollection**  
  Contains and switches between different Pages. The Pages are added at the BP level.

**You can check the Game Widget page [here](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/WidgetSetupGame.md)**.  

- General
  - **General_Page**  
  Handles the switching between active widgets, Active Profile and SubProfile [check save profile], as well as is recommended to be the one with the save/check pending/reset settings functions/buttons.

***

# Blueprint Setup
To set up each widget in the following, you’ll have to correctly inherit from the respective C++ widget class. The blueprint widgets in 1.1 showcase how to use this new system as well as what you will need to get it running.


## General Widget

**Settings_All**: Most of the setup required is the visuals plus the widget switcher as well as simply the button click delegates to save/reset/cancel/check settings/etc. Most custom implementations will be significantly simpler than the one in 1.1 given that this widget has to display all the possibilities you can use, whereas most projects will simply have a “Save settings”, “cancel settings”, and “reset settings”.  

![image](https://user-images.githubusercontent.com/28312571/147324970-57aec9a7-41f0-4a3d-8c64-b13b61b5a17b.png)

__________________________________  

# Examples

> This feature is available on 1.2  

The Point of Example Widgets is to even further the reduce the setup necessary in order to get your custom version up and running. We focus on the big brain stuff, you focus on making it look flashier!  

> As updates roll out, so will more example configurations. If you have any suggestions and/or contributions, send us a message :)  

## Example Implementation

### Advanced Copy

In case you want to simply duplicate an entire configuration for your project to use as a starting point, you can use the Engine's `Advanced Copy` Feature.  

![image](https://user-images.githubusercontent.com/28312571/158045466-5c72cdd4-8c52-499b-b6b0-f7ac6b272735.png)  

Then as you drag it in, select advanced copy. This will copy it with all the references required and they will update to the newly created assets.  

![image](https://user-images.githubusercontent.com/28312571/158045503-6035dd7e-9530-4bb8-823e-14b1632d374e.png)  

> **NOTE**: For 1.2.0, you may spot a couple of lingering references to either V1 or Test Widgets. This should be resolved by 1.2.1.  

Your screen may look something like this. Click Accept and you're ready to go!  

![image](https://user-images.githubusercontent.com/28312571/158045522-7af3fcc6-e355-4f9b-b2e1-d4079850c495.png)  

### Manual

If you want even **more** control, we also recommend inheriting from our widgets directly! Each Template Example will have a specific set of children that you can take a look into inheriting from. If the present examples don't quite fit what you're after, you can inherit from the [Widgets in 1.1](#blueprint-setup) and do their setup as mentioned/shown in the V1_1 folder.  

You can either extend our classes in 
* C++: you can check ``/CM_Engine_Options/Source/CM_Engine_Options/Public/Widgets/` for 1.1 widgets and `/CM_Engine_Options/Source/CM_Engine_Options/Public/Widgets/Templates` (/Default, /Generic/ and other Examples in next updates) for examples on implementation  
* Bp: Follow the Above notes as well as take a look at the Widgets inside V1_1  


## Base Widgets

### Generic Widgets

For the blueprint implementation examples of these, you can refer to `/Content/Examples/Generic` and see just how little you have to do to get your setups working 🤘.  
#### Example Generic 

Prefix for Blueprints: WBP_EO_

* **CM_Engine_Options_Widget_Base_Button** (1.2)  
  Abstracted version of `BP_Test_Options_Widget_Base_Button`. Provides an easy to use Button with plenty of basic options.  
  > Blueprint Version. `WBP_EO_BaseButton`  
  
  ![image](https://user-images.githubusercontent.com/28312571/158045664-aa20386e-6e71-4027-bf68-eb9ac6e08887.png)  

* **CM_Engine_Options_Widget_Title_Button** (1.2)  
  Child class of `CM_Engine_Options_Widget_Base_Button` that adds the value of a category name. We use this for switching between Input Profile Names, as an example.   
  > Blueprint Version. `WBP_EO_TitleButton`  
  
  ![image](https://user-images.githubusercontent.com/28312571/158045743-edcf433d-28a2-4659-8873-99d2e322e998.png)

* **CM_Engine_Options_Widget_Slider** (1.2)    
  Abstracted Version of `WBP_EngineOptions_Slider`. Provides an easy way of setting up a setting Widget with a slider.  
  > Blueprint Version. `WBP_EO_Slider`  
  
  ![image](https://user-images.githubusercontent.com/28312571/158045800-88038d02-470c-4ed3-a5f8-8ac896cacd27.png)

* **WBP_EO_StringSlider** (1.2)  
  Child class of 1.1 widget `Generic String Switch`.  
  
#### Example Default

> This Example Layout is available in 1.2

Prefix for blueprints: WBP_EOD_

* **CM_Engine_Options_Widget_Graphics_Page_Default** : Child Class of `CM_Engine_Options_Widget_Graphics_Page` 
* **CM_Engine_Options_Widget_Graphics_Slot_Button** : Child Class of `CM_Engine_Options_Widget_Graphics_Slot`  
* **CM_Engine_Options_Widget_Graphics_SlotProfile_Button** : Child Class of `CM_Engine_Options_Widget_Graphics_SlotProfile`  
* **CM_Engine_Options_Widget_Input_Slot_Default** : Child Class of `CM_Engine_Options_Widget_Input_Slot`  


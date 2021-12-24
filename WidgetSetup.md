# Modular Widget Setup (1.1+)  

As of 1.1, we do not recommend using the Widgets inside CM_Engine_Options Content/Test. Nevertheless, for reference and legacy purposes, they will remain in the plugin. This document will only cover widget setup and workflow with the widget setup implemented on 1.1+. 

#### Table of contents   
[Hierarchy, Modularity, and C++ magic](#hierarchy)  
- [Concepts](#concepts)  
- [General Convention](#general-convention)  
- [Class hierarchy](#class-hierarchy)     

[Blueprint Setup of Widgets](#Blueprint-setup)
- [Graphics Widget](#graphics-widget)
- [Audio Widget](#audio-widget)
- [Game Settings Widget](#game-settings-widget)
- [Input Widget](#input-widget)
- [General Widget](#General-widget)

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
  - Graphic_Slot -> used for Quality Slot entries, such as Textures/Shadows/etc. Each entry is responsible for/modifies a specific quality setting. 
  - Graphic_SlotProfile -> used for Quality Profile switching which affects all quality types -and therefore is treated as the “global” switcher. Switches in between the different layers that Graphic Profile has [Low/Medium/Custom] etc
  - Graphic_Page -> auto generates Graphic Slot and Profile, and serves as the baseline for storing Graphic-related widgets. Check the Blueprint setup for info. Resolution/Frame Limit/Vsync/among others are added at the BP level.

- Audio
  - Audio_Slot -> used to represent a specific entry of a given Sound Layer. You can have Sound::Volume, Sound::Pitch, and Sound::FadeDuration
  - Audio_Page -> auto generates/detects Audio Slots, and can generate as many widgets as there are settings for it. Check the Blueprint setup for info. If auto generation is turned off, it can auto detect Audio Slot classes added at design time.

- Input
  - Input_Slot -> represents a given binding entry; can be either Action or Axis Binding. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture).
  - Input_OptionsWindow -> is the widget that pops up whenever a binding request is made, either by listening key or manual binding. Is summoned by Input_Slot by the user’s request.
  - Input_KeyEntry -> Is what OptionsWindow uses to fill its dropdown. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture).
  - Input_Page -> Handles the different profile switching, generation, and Input Request handling. Input Profiles present get generated at the BP level. 

- Game Settings
  - Game_Page -> represents an individual Game Profile [for example in the default project, the Gameplay Profile that contains Perma Death and Mouse Multiplier settings]. The Individual Slots are added at the BP level.
  - Game_PageCollection -> Contains and switches between different Pages. The Pages are added at the BP level.

- General
  - General_Page-> Handles the switching between active widgets, Active Profile and SubProfile [check save profile], as well as is recommended to be the one with the save/check pending/reset settings functions/buttons.

***

# Blueprint Setup
To set up each widget in the following, you’ll have to correctly inherit from the respective C++ widget class. The blueprint widgets in 1.1 showcase how to use this new system as well as what you will need to get it running.

## Graphics Widget

**Graphics Page**: We strongly recommend users leaving the auto generated options available. Users can swap widget classes with their own in default panel
We have left things like Resolution/Frame Limit/Window Mode/etc at the blueprint level to allow more freedom at the design level given that those can be implemented in many different ways. 

![image](https://user-images.githubusercontent.com/28312571/147323853-cbd4ba0b-de91-4d6b-a5fe-dd15ce25bbd2.png)

You only need to set up the widget layout to your needs, plus set the send functions + override the update parameter functions. Bool Parameters include things like Vsync, float parameters include Frame rate, and string parameters include Resolution

![image](https://user-images.githubusercontent.com/28312571/147323867-7e570d08-2a7e-4219-96a9-86d00a03d922.png)

**Graphics Slot & Profile:** Only requires Graphical layout plus a couple of visual overrides + send functions
![image](https://user-images.githubusercontent.com/28312571/147323914-d35b98de-240c-43fe-81e0-eb7dcd00daea.png)

> Comparison between the 1.0 and 1.1 widgets 

![image](https://user-images.githubusercontent.com/28312571/147323932-59c1a4f6-fd1a-4086-a3a6-dc90b4985df5.png)


## Audio Widget

**Audio Page:** We strongly recommend leaving the auto generated options enabled. If you wish to disable auto-gen, you can add your Audio Slot widgets at design time; the Construct Event in C++ will auto detect them if bAutoGenerate is false. When adding manually, set the sound class name and sound type at each slot level. Do not add them at runtime, as the system will not add them to the respective important events.

![image](https://user-images.githubusercontent.com/28312571/147324123-e08d21f2-92f6-4fb5-afe3-d7c0f8164fd0.png)

**Audio Slot:** You need to set the visual elements plus a couple of visual update functions. You also need to override the get function to update your visual slider :). 

![image](https://user-images.githubusercontent.com/28312571/147324182-ff470372-6b39-4042-9a3a-65c44a7a08d2.png)


![image](https://user-images.githubusercontent.com/28312571/147324219-61a785cb-3af1-43f3-b20b-6b93b0da30df.png)


## Game Settings Widget

**Game Settings_Page [Collection]**: You need to set up the different pages that participate as well as the Switch functions. 

![image](https://user-images.githubusercontent.com/28312571/147324262-d31dbd1c-9331-426f-8a45-eb0c123d4203.png)

**GamePage [Each]** : You need to set up the visual elements plus set the adequate Update Values [Float/Byte/String] via GameSettingsChange[Type]. You can configure your settings at the defaults panel.

![image](https://user-images.githubusercontent.com/28312571/147324340-4c0f8a7d-b03c-42d5-b119-865ff1cb41bb.png)

To update your values, you’ll need to override the adequate update visual functions, where you can check the name of the setting and update the visual elements.   

![image](https://user-images.githubusercontent.com/28312571/147324371-cbbeb562-2386-455b-a29c-8f4d643e2f8d.png)
 
![image](https://user-images.githubusercontent.com/28312571/147324387-37f6f06c-1eaf-4682-a424-8dba6f49732b.png)


## Input Widget

**InputKey Entry:** Design required [a simple text and texture values + the SetInputContentX functions overridden.]  
![image](https://user-images.githubusercontent.com/28312571/147324508-729d4602-eb19-4811-adb8-3b1a1e1583fe.png)

**Input Slot** : The graphic setup, overriding the SetInputX functions for setting text/textures [like in Input Key entry], as well as a couple of request sending + Visuals updating
![image](https://user-images.githubusercontent.com/28312571/147324545-02a0c406-ec69-400e-8b30-741a6236248b.png)

The Input Widget Slot Fill text dictates how the content should attempt to be filled   
![image](https://user-images.githubusercontent.com/28312571/147324574-cba60296-dbbd-4dd2-aa98-5fb6cc2985a1.png)

**Input Options Window** - This one does require a bit of setting up, especially visually given the options that it has based on the Binding type & Results. The base bp widget has the following structure:
![image](https://user-images.githubusercontent.com/28312571/147324622-36b707ff-8ab7-4acb-a8bc-5e43cb7d3b5c.png)

We also need to run a few functions for sending requests as well as handling the visuals to achieve nicer results. That said, there are only 2 setups that matter. For sending the manual requests:  
![image](https://user-images.githubusercontent.com/28312571/147324671-1c013585-5794-4517-ba6a-63a4cdf104a9.png)

The second part: widget creation  
![image](https://user-images.githubusercontent.com/28312571/147324704-27be2673-32af-49c2-aa92-3afb2d4f9b53.png)  
![image](https://user-images.githubusercontent.com/28312571/147324718-13f3e82c-5492-42ce-8094-b1d6b3119f5f.png)

This way we’ll be able to spawn our custom widget instead of just having the classical generic string widget that ComboBoxString offers.  
![image](https://user-images.githubusercontent.com/28312571/147324749-9b133ff0-54a9-44b4-ad40-c34cad476869.png)
  
**Input Page:** Requires very little visual setup, along with a function registration. Example Layout:
![image](https://user-images.githubusercontent.com/28312571/147324783-6d91b5ac-4aa2-4e73-92a9-4f30db269d23.png)

You also have the Input Generation Page, which determines in which order the Input Keys are added if we use bAutoAddInputSlotsToScrollBox. Not using this slot allows you to add them manually [for instance, based on category] at the OnInputSlotAdded Delegate. You need to set the Row Class for Input Slot. The only other thing that projects with multiple input profiles [changeable at the widget level] may require is input switching. The default setup has some auto generated profiles that you can check out. 
![image](https://user-images.githubusercontent.com/28312571/147324803-86002e00-880f-455c-83ae-a13aabe85eef.png)  

To change a profile and have everything handled for you, just run the subsystem function Input Set Profile. Example from the default implementation.  
![image](https://user-images.githubusercontent.com/28312571/147324862-82e8fcfa-b744-4984-9504-959ef3fec771.png)  

Finally, we register our window widget to the page   
![image](https://user-images.githubusercontent.com/28312571/147324902-177e8226-3e2a-4f60-82b6-9111637a9315.png)

> Comparison between the 1.0 and 1.1 Widgets  

![image](https://user-images.githubusercontent.com/28312571/147324922-9943fc9f-0772-4adf-ae93-0da9dc57af95.png)


## General Widget

**Settings_All**: Most of the setup required is the visuals plus the widget switcher as well as simply the button click delegates to save/reset/cancel/check settings/etc. Most custom implementations will be significantly simpler than the one in 1.1 given that this widget has to display all the possibilities you can use, whereas most projects will simply have a “Save settings”, “cancel settings”, and “reset settings”.  

![image](https://user-images.githubusercontent.com/28312571/147324970-57aec9a7-41f0-4a3d-8c64-b13b61b5a17b.png)



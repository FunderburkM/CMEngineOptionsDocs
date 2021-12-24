# Engine Options Subsystem and Implementation

#### Table of Contents  

[General Data](#General)  
[Implementation](#Implementation)
[Technical](#Technical)

# General  

**Blueprint Exposure for Structs**  
Struct Functions are exposed by the function libraries:
- UCL_Engine_OptionsLibrary
- UCL_Engine_OptionsGraphicsLibrary
- UCL_Engine_OptionsInputLibrary  

> For general functions, we generally recommend to not use them directly, and instead utilize the functions inside the subsystem when possible.    
    This is especially true for Input Bindings, as the exposed Rebinding keys are not “World aware,” and do not look for overrides/switches/errors; they communicate directly with the user settings. 

# Implementation 

The whole system is brought together by the ULocalPlayerSubsystem ULPSS_Engine_Options. This is the class with the Delegates, Functions, Save Connection Mechanisms, etc. 

**We encourage only this class’ functions to be used for major functionality.**

In this example project, we implement things as modularly as possible, setting up basic widgets with delegates that then get executed at the merging UMG level. 

## Saving
The Save Game is implemented by the class USG_Engine_Options. Please do not use Directly, and instead only use the functions inside the subsystem

The main global functions for handling saving functionality as a whole are as follows:
- Settings_LoadApplySaved -> Will load and Apply the currently Saved / Default Settings
- Settings_ResetSetting -> Will load and Apply the Defaults (by Project Config). bSave = true affects the SaveGame
- Settings_Apply_PendingSave -> This function will Apply any and all potential settings that haven't been saved
- Settings_Check_PendingSave -> This checks if there are any unsaved changes.

## Function Calling  

To easily fetch the subsystem, you can call `ULPSS_Engine_Options::GetOptionsSubsystem(Ubject* ContextWorldObject);` 

Each major module has its own function prefix. 

- Settings_ -> Global / Admin functions
- Graphics_ -> Graphic-related functions
- Input_    -> Input control-related functions
- Game_     -> Game settings-related functions
- Audio_    -> Audio settings-related functions

Functions without a prefix are getters/specific functions used internally.   
You can check either LPSS_Engine_Options.h or BP_Documentation for a list of functions and delegates.
![](https://user-images.githubusercontent.com/28312571/147325436-f71e257e-237a-4dce-acdf-d33de5c2e940.png)

## Field of View

The Field of View is implemented via the interface ICI_Engine_Options_Camera. The subsystem checks both in the PlayerController’s Actor and Components as well as the Controlled Pawn’s Actor and Components for an Object that implements the interface. _You can check the example implementation at Content/Text/BP_Test_Options_Pawn._

The system will look for the Object implementing the interface in the following Classes:
- The Player controller
- The Controlled Pawn
- The Current Camera Manager
- The Current View Target (Via Playercontroller->CameraManager)

>The Object implementing the interface can be either the Actor in question or any of its Components!  

## Hooking up with Game Settings  

The plugin contains an example with blueprints. The equivalent C++ code would be as follows: 

```cpp
void AMyActor::BeginPlay()
{

  Super::BeginPlay();
  ULPSS_Engine_Options* EngineOptions= ULPSS_Engine_Options::GetOptionsSubsystem(this);
  if (EngineOptions != nullptr)
  {
     //If it's just bool properties, you'll probably only want byte settings. You also have Float and String Settings though :)
     EngineOptions->GameSettingByteUpdateDelegate.AddDynamic(this, &AMyActor::GameSettingUpdate_Byte);
     //You want to set Engine Options somewhere so that you don't have to keep Querying for it :) 
     MyEngineOptions = EngineOptions;
     //Let's update everything on beginplay so that we're synced
     GameSettingUpdate_Byte(FEngineOptionsStaticNames::NAME_All, FEngineOptionsStaticNames::NAME_All);
  }
}

void AMyActor::GameSettingUpdate_Byte(FName ProfileName, FName SettingName)
{

  if (MyEngineOptions == nullptr)
  {
    return;
  }

  if (ProfileName== FEngineOptionsStaticNames::NAME_All || SettingName == FEngineOptionsStaticNames::NAME_All)
  {
      //All values have been updated. I should for loop here to fetch all the values that I would like to fetch  
  }
  else
  {
    //Here I can compare if the Profile or SettingName element that got updated is one that MyActor listens to. for example, you can have a Map or an Array that contains these at the actor level :) 
    //if we do listen to that parameter, let's fetch it! 
    const uint8 Fetched = Options->Game_GetSettings_Byte(ProfileName, SettingName);
   
    //We've fetched the value. We can now do with this what we want
  }

}

```

***

# Technical  

- **ULPSS_Engine_Options** is the Player Subsystem that holds everything in place, handling the different elements, their interaction, and handles broadcasts for others to listen to.
- **USG_Engine_Options** is the save game that stores the data locally.
- **FEngineOptionsCurrentControllerData** implements the controller & device connected and recognized information.

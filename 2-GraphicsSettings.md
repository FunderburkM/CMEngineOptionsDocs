# Graphic Settings

## Features

* Set Scability Settings for the Game  
* Set Resolution and Window Mode  
* Set Gamma, Vsync, and Dynamic Resolution  
* Enforce Framerate and Monitor Synchronization  
* Switch Active Monitors (v2.1+)  

## Instanced Settings

![Graphics](/Resources/Assets/SS_SettingsAsset_Graphics.JPG)  

The graphics Asset contains  

* `Graphic Name Settings`, Name-Text map used for display purposes  
* `Default Profile Name` controls which graphic profile should be active by default when not loaded by save game (And when auto settings are not set to apply)  
* `Auto Detect Configuration` controls the auto detect configuration and loading settings  
* `Resolution Settings` and `Screen Settings` give the default settings for values  
* `Graphic Profiles` contain each profile present and what their settings are, also their individual setting type.  
  * `Locked` profiles cannot modify individual graphic types  
  * `Within range` profiles can modify individual graphic types, and use `Cycle settings` to control if there are any blocked settings. For example, you may never want your post process to go below `Low`.  
  * `Unlocked` allows each type to be modified without restrictions.  

![Graphics Asset](/Resources/Assets/SS_GraphicsAsset_Default.JPG)  

## Functionality

You can check the API for Graphic Settings by doing  

```cpp

if (UUOGraphicSettings* Settings = UUOGraphicSettings::Get(this))
{
    //For Example, getting if Vsync is enabled
    Settings->GetVSync();
    //Then applying it
    Settings->SetVSync(true, UUOCoreHelper::GetEvent_ApplySave()));
    //Prefer to use the Gameplay Tag Container options in CoreHelper in C++,
    // and use the Container Builder default options in Blueprint
}
```  

By Default, The Graphics Functionality at runtime is triggered by the Active Graphic Settings Widget.  

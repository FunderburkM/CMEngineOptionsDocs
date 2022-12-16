# Understanding Assets

![Settings Defaults](/Resources/Framework/SS_SettingsAsset_Minimzed.JPG)  

* **Widget Settings** Asset controls our automatic Widget Settings  
* **Graphic Settings** Instanced object controls the graphics-related data, features, and settings  
* **Sound Settings** Instanced object controls the sound/audio-related data, features, and settings  
* **Input Settings** Instanced object controls the input/rebinding-related data, features, and settings  
* **Game Settings** Map of Tags and Instanced objects control the different active game settings  

## Graphics

The settings at the default asset level dictate which graphics asset to use and which features are enabled.  
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

## Sound Settings

The settings at the default asset level dictate which sound asset to use and which features are enabled.  
![Sound](/Resources/Assets/SS_SettingsAsset_Sound.JPG)  

The Sound Asset contains  

* `Sound Mix` to be used at runtime for administrating our Sound Profiles  
* `Sound Profiles` the different sound classes active and their respective settings  
![Sounds Asset](/Resources/Assets/SS_SoundAsset_Default.JPG)  

## Input Settings - Standard

The settings at the default asset level dictate which standard input asset to use and which features are enabled.  
![Input Standard](/Resources/Assets/SS_SettingsAsset_InputStandard.JPG)  

Rebind Standard Input interacts with the Engine's regular Input System, which you can find by going to `Project Settings -> Engine -> Input`

The Input Asset contains  

* `Input Binding Display Options` used by widgets, it lets us define the text values for each binding, both action and axis.  
* `Input Layouts` control the main element on binding (more below)  
* `Input Profile` controls which input profile should be active by default when not loaded by save game  
* `Non Rebindable Keys` establishes with Keys (Gamepad Left, Global Back, Escape Button, etc) cannot be accepted for rebinding.  
* `Non Rebindable Mappings` establishes with mappings (by Name, so say "Escape" action binding) cannot be opened for rebinding.  
* `Input Profiles` establishes the settings for each individual input profile (more below)  

![Input Asset](/Resources/Assets/SS_InputStandardAsset_Default.JPG)  

### Input Layouts

This Section will be filled soon.  

### Input Profiles

## Input Settings - Enhanced

This feature will be implemented soon.  

## Game Settings

This Section will be filled soon.  

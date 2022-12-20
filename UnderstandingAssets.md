# Understanding Assets


Our Settings are based on data assets to ensure data driven design. The general philosophy is that each setting type (say graphics or audio) will have two Data Asset Types: System Settings and Widget Settings.  
![image](/Resources/Assets/SS_AssetPicker_Duo.JPG)  

The `Global Widget Settings` asset stores the widget information, and you can check more [here](/WorkingWithWidgets.md). For this page, let's focus on the system settings, starting on our Settings Asset:  

![Settings Defaults](/Resources/Framework/SS_SettingsAsset_Minimzed.JPG)  

* **Widget Settings** Asset controls our automatic Widget Settings  
* **Graphic Settings** Instanced object controls the graphics-related data, features, and settings  
* **Sound Settings** Instanced object controls the sound/audio-related data, features, and settings  
* **Input Settings** Instanced object controls the input/rebinding-related data, features, and settings  
* **Game Settings** Map of Tags and Instanced objects control the different active game settings  

To make your own Settings Asset, click Add -> Miscellaneous -> Data Asset  
![image](/Resources/Assets/SS_AssetPicker_Settings.JPG)  

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

An Input layout is a contained configuration for accepting and determining what keys are both valid to be used and what their display settings are, for example if you wanted to show the interaction icon to switch depending on which input profile and device is connected. Input layouts are useful for display information, but even more crucial for rebinding. Whenever `bRestrictInputLayoutToKeyData` is true, keys not found in the given key display data map will not be accepted.  

These layouts declare their name, profile type (keyboard, gamepad, all), and which key data to use for functionality purposes.

### Input Profiles

Input profiles are the actual collections of bindings that get used. Profiles reference Layouts via their `LayoutProfileName` variable to understand what input layout they follow, as well as settings to either autofill what is in the project's input settings configuration ini or whether to set up the manual input bindings either at design time or runtime.  

Input Profiles also give you options for rebinding, such as conflict resolution, key interaction and swapping options.  

## Input Settings - Enhanced

This feature will be implemented soon.  

## Game Settings

The settings for Game work on establishing a Gameplay Tag map pointing to an instanced object of `UOGameSettings` type. Each key determines a given _type_ of game setting for our systems to differentiate and choose.  

![Image](/Resources/Assets/SS_SettingsAsset_Game.JPG)  

Your defaults can be set at either the object class that you choose to or alternatively modify the exposed defaults of said class at a settings level, too. For information on how this functions, check [creating your own game settings](/CreatingYourOwnGameSettings.md).  

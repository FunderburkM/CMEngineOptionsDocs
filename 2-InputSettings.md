# Input Settings

## Features

### Input Layouts

An Input layout is a contained configuration for accepting and determining what keys are both valid to be used and what their display settings are, for example if you wanted to show the interaction icon to switch depending on which input profile and device is connected. Input layouts are useful for display information, but even more crucial for rebinding. Whenever `bRestrictInputLayoutToKeyData` is true, keys not found in the given key display data map will not be accepted.  

These layouts declare their name, profile type (keyboard, gamepad, all), and which key data to use for functionality purposes.  

___  

## Standard Input

### SI Features

* Input Rebinding for Common Input  
* Input Profiles with auto detection settings  
* Rebinding supports Modifier Keys  

#### SI Input Profiles

Input profiles are the actual collections of bindings that get used. Profiles reference Layouts via their `LayoutProfileName` variable to understand what input layout they follow, as well as settings to either autofill what is in the project's input settings configuration ini or whether to set up the manual input bindings either at design time or runtime.  

Input Profiles also give you options for rebinding, such as conflict resolution, key interaction and swapping options.  

### SI Instanced Settings

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

___  

## Enhanced Input

Enhanced Rebinding is available in v2.1+.  

___  

## Functionality

You can check the API for Input Settings by doing  

```cpp

if (UUOInputSettings* Settings = UUOInputSettings::Get(this))
{
    //For Example, getting the profile name
    Settings->GetProfileName();
    //Setting it to a new one
    Settings->SetActiveProfile("Ttest", UUOCoreHelper::GetEvent_ApplySave()));
    //Prefer to use the Gameplay Tag Container options in CoreHelper in C++,
    // and use the Container Builder default options in Blueprint

    //To check standard vs enhanced
    if (auto* StandardInput = Cast<UUORebindStandardInput>(Settings))
    {

    }
    //v2.1+
    else if (auto* EnhancedInput = Cast<UUOEnhancedRebindInput>(Settings))
    {

    }
}
```  

By Default, The Input Functionality at runtime is triggered by the Active Input Settings Widget.  

# Game Settings

## Features

* Endlessly expansive by the developer  
* Have as many settings as you want to, all encapsulated by concept  

## Instanced Settings

The settings for Game work on establishing a Gameplay Tag map pointing to an instanced object of `UOGameSettings` type. Each key determines a given _type_ of game setting for our systems to differentiate and choose.  

![Image](/Resources/Assets/SS_SettingsAsset_Game_New.JPG)  

**Deprecated Settings** Where you had to specify both the tag and the object being loaded. This is rather redundant, but this legacy behavior is still supported.  

![Image](/Resources/Assets/SS_SettingsAsset_Game.JPG)  

Your defaults can be set at either the object class that you choose to or alternatively modify the exposed defaults of said class at a settings level, too. For information on how this functions, check [creating your own game settings](/3-CreatingYourOwnGameSettings.md).  

## Functionality  

You can check the API for GameSettings Settings by doing  

```cpp

if (UUOGameSettings* Settings = UUOGameSettings::Get(this, SomeGameSettingsGameplayTag, SomeGameSettingsClass))
{
    //For Example, getting the profile name
    FUOJsonValue OutValue;
    if (Settings->GetData(TEXT("$.mySaveGameVariable"), OutValue))
    {
        //Read/Convert
    }
}
```  

By Default, The Game Setting Functionality at runtime is triggered by the Active Game Settings Settings Widgets, but you are more than encouraged to have game settings that aren't exposed to widgets as your needs may fit.  

## Registering Game Settings at Runtime

Game settings don't have to be baked into the settings asset up front. You can add them while the game is running — useful for Game Feature plugins, DLC, or mode-specific options that should only exist while that content is active.

* `UUOUniversalOptions::RegisterGameSetting` / `UnregisterGameSetting` — add or remove a single game setting by tag/class. Registration returns an `FUORegistrationHandle` you keep and pass back to unregister.
* `RegisterSettingsFragment` / `UnregisterSettingsFragment` — register a whole fragment (a group of settings) in one call, Game-Feature style.
* `OnGameSettingRegistered` / `OnGameSettingUnregistered` — subsystem events so widgets and listeners can react to the catalog changing.

Registered settings also survive a `RestartSystemWithAsset` by default, so hot-swapping the base asset won't drop dynamically added content.  

### From a Game Feature (no code)

If your content ships as a Game Feature plugin, you don't have to call the registration functions yourself. Add the **`Add Settings Fragment`** action (`UOGameFeatureAction_AddSettingsFragment`) to the Game Feature's action list and point it at a `UUOSettingsFragment` — a small asset that lists the game settings that feature contributes. When the feature activates, its settings are registered; when it deactivates, they're removed. This is the recommended path for DLC- or mode-scoped settings, since their lifetime is tied to the feature's own.  

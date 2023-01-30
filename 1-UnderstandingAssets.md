# Understanding Assets

Our Settings are based on data assets to ensure data driven design. The general philosophy is that each setting type (say graphics, input, audio, game settings) will have two Data Asset Types: System Settings and Widget Settings.  
![image](/Resources/Assets/SS_AssetPicker_Duo.JPG)  

The `Global Widget Settings` asset stores the widget information, and you can check more [here](/1-WorkingWithWidgets.md). For this page, let's focus on the system settings, starting on our Settings Asset:  

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

Check [Graphic Settings Page](/2-GraphicsSettings.md) for more information.  

## Sound Settings

The settings at the default asset level dictate which sound asset to use and which features are enabled.  
![Sound](/Resources/Assets/SS_SettingsAsset_Sound.JPG)  

Check [Sound Settings Page](/2-SoundSettings.md) for more information.  

## Input Settings

The settings at the default asset level dictate which standard input asset to use and which features are enabled.  
![Input Standard](/Resources/Assets/SS_SettingsAsset_InputStandard.JPG)  

Check [Input Settings Page](/2-InputSettings.md) for more information.  

## Game Settings

The settings for Game work on establishing a Gameplay Tag map pointing to an instanced object of `UOGameSettings` type. Each key determines a given _type_ of game setting for our systems to differentiate and choose.  

![Image](/Resources/Assets/SS_SettingsAsset_Game.JPG)  

Your defaults can be set at either the object class that you choose to or alternatively modify the exposed defaults of said class at a settings level, too. For information on how this functions, check [creating your own game settings](/3-CreatingYourOwnGameSettings.md).  

Check [Game Settings Page](/2-GameSettings.md) for more information.  

# Sound Settings

## Features

* Control Active Sound Classes and their volume, pitch, and fade time.  
* Change Audio Output Device  (v2.1+)  

## Instanced Settings

The settings at the default asset level dictate which sound asset to use and which features are enabled.  
![Sound](/Resources/Assets/SS_SettingsAsset_Sound.JPG)  

The Sound Asset contains  

* `Sound Mix` to be used at runtime for administrating our Sound Profiles  
* `Sound Profiles` the different sound classes active and their respective settings  

![Sounds Asset](/Resources/Assets/SS_SoundAsset_Default.JPG)  

## Functionality

You can check the API for Sound Settings by doing  

```cpp

if (UUOSoundSettings* Settings = UUOSoundSettings::Get(this))
{
    //For Example, getting the music volume
    Settings->GetProfileValue("Music", EUOSoundSetting::Volume);
    //Setting it to a new one
    Settings->SetProfileValue("Music", EUOSoundSetting::Volume, 1.f, UUOCoreHelper::GetEvent_ApplySave());
    //Prefer to use the Gameplay Tag Container options in CoreHelper in C++,
    // and use the Container Builder default options in Blueprint


}
```  

By Default, The Sound Functionality at runtime is triggered by the Active Sound Settings Widget.  

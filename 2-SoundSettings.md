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

## Output Device Switching

When output device switching is enabled (`bEnableOutputDeviceSwitching` on the instanced settings), you can list and change the audio output device at runtime:

* `GetAudioDevices` / `GetAudioDeviceNames` — enumerate the available devices for a dropdown.
* `SetActiveDevice` / `SetActiveDeviceByName` — switch to one (takes the usual Event container).
* `GetActiveDevice` / `GetDeviceIndex` — read the current selection.
* `IsAllowedToSwitchAudioDevices` — whether the running platform permits it (driven by [Platform Settings](/2-PlatformSettings.md)); use it to hide or disable the widget where switching is not allowed.

The device list is queried asynchronously, so bind these events instead of polling:

* `OnSoundDeviceListRefreshed` — fires when the available-device list is (re)built. Rebuild your dropdown here.
* `OnSoundDeviceChanged` — fires when the active device actually changes.

`CallAndRegisterOnSoundDeviceListRefreshed` binds a callback and immediately delivers the current list, which is convenient when a widget is created after the list already resolved.  

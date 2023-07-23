# Platform Settings

Platform Settings data get specified in our active Settings  

![Platform Section in UO Settings Asset](/Resources/Platform/PlatformSettingsAsset.JPG)  

You can make a new Platform Settings asset by making a new Data Asset from type:  
![Asset](/Resources/Platform/PlatformSettingsAssetType.JPG)  

To apply it, set the data asset value in your active [Setting Asset](/README.md#loading-settings).  

Platform Settings allows us to set how the plugin should behave depending on the detected platform that the project is currently running on.  

You can test these settings in editor by going to Project Settings -> Universal Options and turning these Simulation settings on:  

![Init](/Resources/Platform/PlatformSettings_Init.JPG)  

## Graphic Settings

### Auto Detect

You can control which platforms and which graphic architectures auto-detect is allowed to run on.  

### VSync and Frame Rate

You can also disable which platforms should not be allowed to mutate vsync settings via the plugin. This also applies to Variable Framerate, with either the option of Set  

![VSyncEnable](/Resources/Platform/PlatformSettings_VSyncFrameRate.JPG)  

### Resolution Management

You can also prevent the plugin from being able to control the resolution of the device that the game is running on. Resolution on Launch permission sets the resolution of the game whenever the plugin first initializes. Resolution Afterwards allows the plugin to modify the resolution at any point afterwards. This is the desired behavior if you want the user to be able to change the resolution say via the options menu.  

![AllowedResolution](/Resources/Platform/PlatformSettings_ResolutionPermission.JPG)  

### Disabled and Overridden Graphics

You can also specify any given graphics quality type that the plugin should not be able to affect.  

Likewise, you can also set specific graphic configurations for different platforms. Say you want IOS and Android to only ever be in a Custom/Medium level, but Desktop (Windows, Mac, Linux) should still have access to being able to change them.  

![Overrides](/Resources/Platform/PlatformSEttings_GraphicOverride.JPG)  

## Widget Visibility

You can also specify which platforms should widgets like graphic quality not be visible as well as which widget categories to straight up hide, overriding any settings in the [Global Widget Settings](/3-WorkingWithWidgets.md).  

![Visibility](/Resources/Platform/PlatformSettings_Widgets.JPG)  

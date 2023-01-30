# Universal Options - Docs

[Unreal Marketplace Profile](https://www.unrealengine.com/marketplace/en-US/profile/M+Funderburk).  
[Discord Server](https://discord.gg/QHTTMQ6Pqw).  

Documentation for the Universal Options Marketplace Plugin, V2! Available for Unreal Engine 5.  

**These Docs are for Universal Options V2.1**.  
[For documentation on v2.0 version of this plugin, visit this branch.](https://github.com/FunderburkM/CMEngineOptionsDocs/tree/V2.0-Docs)  
[For documentation on v1.x version of this plugin, visit this branch.](https://github.com/FunderburkM/CMEngineOptionsDocs/tree/V1-Docs)  

![Image](/Resources/Game/SS_Graphics_UI.JPG)  

---  

This Readme focuses on the basic elements like plugin enabling, content location, and project settings. For more information, please refer to the following pages:  

* 0) [Version Changelog](/0-ChangeLog.md)  
* 1) [Getting Started](/0-GettingStarted.md)  
* 1) [How Settings Work](/1-HowSettingsWork.md)  
* 1) [How Saving Works](/1-HowSavingWorks.md)  
* 1) [Understanding Assets](/1-UnderstandingAssets.md)  
* 1) [Working With the Settings System](/1-WorkingWithTheSettingsSystem.md)  
* 1) [Working with JSON](/1-WorkingWithJson.md)  
* 2) [Game Settings](/2-GameSettings.md), [Graphic Settings](/2-GraphicsSettings.md), [Input Settings](/2-InputSettings.md), [Sound Settings](/2-SoundSettings.md)  
* 3) [Working with Widgets](/3-WorkingWithWidgets.md)  
* 3) [Creating your own Game Setting](/3-CreatingYourOwnGameSettings.md)  

## Content

The Plugin's technical name is `CM_Engine_Options`, and its friendly name is `Universal Options`. Finding the plugin by folder will be using its technical name, either `CM_Engine_Options`, or `CMEngineOptions`, whilst its display name inside the editor will follow its friendly name.  

To enable the plugin, go to Plugins window and search for Universal Options and enable the plugin.  
![Plugin Window](Resources/Basics/SS_PluginWindow.JPG)  

You can find the files and content inside the Plugin Directory - Universal Options.  
![Content Browser](Resources/Basics/SS_ContentBrowser_Content.JPG)  

All uassets related to V1.x elements have been moved to `Content/Deprecated` as shown above. `Content/General Content` contains demo textures and sound files for our test setup. `Content/v2 Content` is where we'll be working with. `Map_Test_Options` is the test map for v1.x, and `Map_Test_Options_V2` for the latest.  

Inside `v2 Content`, we'll see our main Data assets for settings and widget settings.  
![V2 Content](Resources/Basics/SS_ContentBrowser_V2Content.JPG)  

## Loading Settings

Before we continue, let's explain how to find them. Go to your Project Settings, and scroll down to Plugins section. `Universal Options (Deprecated)` is for the V1.x settings, and `Universal Options` is for our active settings.  
![Plugin Bar](Resources/Basics/SS_ProjSettings_Bar.jpg)  

For V2 loading settings, we declare our functionality at the data asset level. Here, you define your data asset to use and the save game name.
![Loading](Resources/Basics/SS_ProjSettings_Loading.JPG)  

### Initialization Settings

You can specify in which circumstances should modules get disabled. For example, while testing in Editor or in debug mode, you may not want the graphic system to automatically load and apply graphical settings.  
![Initiation](Resources/Basics/SS_ProjSettings_Init.JPG)  
![Initiation2](Resources/Basics/SS_ProjSettings_Init2.JPG)  

### Backwards Compatibility

You can control which version gets loaded (v1.x or v2) by going in the Deprecated Settings and checking this option:  
![V2 Setting](Resources/Basics/SS_ProjSettings_Deprecated.JPG)  

We can also check and recover for V1 save files that your project may have already! In the `Universal Options` section, head down to `Backward Compatibility` category.  
![Backwards](Resources/Basics/SS_ProjSettings_Backwards.JPG)  

The system is controlled by the `V1 Recovery Modules`, a bit flag where you can control which **saved** modules from V1 do we try to convert to V2.  
![Backwards 2](Resources/Basics/SS_ProjSettings_Backwards-2.JPG)  

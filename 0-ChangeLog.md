# ChangeLogs

## v2.3  

New & Improvements (General):  

* Added Systems Settings Category, which lets the developer interact with the save file, handle redirects, among other features. Read [Here](/2-SystemSettings.md).  
* Massively extended the Platform Settings Options. They now cover more Graphics settings, Audio settings, and Input Settings. Check the [Platforms Page](/2-PlatformSettings.md)  for more information.   
* Added the platform recognized as Steam Deck support for recognition, testing, and platform settings.  
* Refactored Game Settings getting added to Settings Asset, reducing the redundancy in setting them up.  
  Added Data Validation Queries to ensure correct settings.  
* Added a range system checker for settings.  
* Removed Asset Manager checks. The system no longer needs asset manager registration to load settings on packaged builds.
* Added a new Listener system where any object can subscribe to System Events without having to implement an interface. Check `UUOUniversalOptionsHelper::RegisterSettingListener`.     
* Restructured the C++ file structure to be navigated in a more adequate way.  
* Abstracted a lot of previously specialized logic into Function Libraries (Check for the UO Helper Types).  
* Exposed a lot more getters, queries, and utility functionality to C++.  
* Added JSON setup support for SetData on graphic settings, audio settings, and input settings. C++ developers can access this via AddInternalParseJsonData() in MySettings::Initialize_Implementation().  
* Extended JSON support for both BP and C++ usage.  
* Fixed a bug with the query system not parsing paths properly when going into arrays.  

New & Improvements (Widgets):  

* Introduced the Widget Builder Template system for system Widgets. Check the [Widget Page](/3-WorkingWithWidgets.md) for more information. The Old widget system still works - it's just been moved to deprecation.   
* Cleaned up Widget Relationships. Most sub widgets with C++ parent types have been decoupled and can now be added individually to any widget instead of having to exist specifically in a wrapper widget defined in code.  
* The new Widget builder system adds supports mixing widgets from different settings in a single panel!  
* The new Widget builder system automatically handles conditional additions to the system via the Widget Condition Validation Feature. Check the [Widget Page](/3-WorkingWithWidgets.md) details. 

New & Improvements (Graphics):  

* Added Graphic Setting Ranges for Gamma, FOV, Res Scale.  
* Graphic settings now get applied on first world startup, not first world beginplay. This fixes hitches and any incongruencies that were seen if the first world took more than a few seconds to load here the screen would resize weird at times.  
* Fixed bugs with Graphic settings not applying properly on occassion.  

New & Improvements (Audio):  

* Added Audio Setting Ranges for Volume per profile.  
* Added Autio Widget settings for creation at the Audio settings and per platform settings.  
* Fixed a bug with Audio device switching not reseting properly after being modified.  
* Fixed a bug with Audio device switching [when saving audio device is turned on] changing to the wrong device. 

New & Improvements (Input):  

* Input Controller Auto Detection is BACK! You can check `UUOInputSettings::OnInputControllerTypeChanged`. You can also set up auto input profile change settings in the input settings asset.  
* Dropped support requirement for the Raw Input Plugin. Users can still use it if they choose to, but using `Universal Options` no longer requires it for controller detection.  
  The Old Detection on Windows settings are still present but no longer do anything. They will be removed for v2.4.  
* Preparation for supporting multi input switching supports in a single widget (coming v2.4)   
* Fixed a bug with Enhanced input rebinding not clearing up duplicate key names being saved. Settings should reset and the system will now work as it should.  
* Fixed a bug with Input Settings not rebuilding logic after changing profiles.  


## V2.2  

New & Improvements:  

* Added support for new Graphic Quality types:  
  * Global Illumination  
  * Reflections Quality  
  * Shading Quality  
* Widget settings now automatically detect the first available system and switch to that instead of a hard coded tag  
* Widget settings improved visibility  
* Graphic, Audio, and Universal Settings UX improvements, availability and query functions added  
* Added Platform Settings, [check here](/2-PlatformSettings.md)  
* Added Platform Simulation and Detection Settings  
* Audio and Graphic Settings now have ensure config validation when loading from save file  
* Graphic Settings can now be overridden per platform  
* Added the ability to restart the system at runtime, useful for benchmarking and working with Gauntlet.  

Fixes:  

* Fixed Erronous detection for Antialiasing and Updated to user Settings instead of console commands.  
* Fixed an input switch not working  
* Widget delegate subscription ensure when using under common UI.  
* Fixed a crash on audio Detection for device switching  
* Fixed a crash on auto graphics detect on Vulkan. There is a separate issue in the 5.1 engine that was fixed in 5.2, but the issues on the plugin side have been fixed for both 5.1 and 5.2.  
* Fixed a loading issue with Graphic Settings where the graphic profile when loaded via auto detect wasn't working as intended  
* Fixed widget building settings and priority with graphic widget types when auto detect ran at runtime, resulting in the list getting generated over and over.  


## V2.1  

* Added support for Enhanced Input Rebinding  
* Added Audio Output Switching Support  
* Save game variables automatic support added  
* UX improvements for both C++ and Blueprints  

## V2.0  

* Completely new system introduced.  
* Instanced Object, Data Oriented, Composition based Settings Framework  
* JSON based Settings  
* GameplayTag and Inheritance based extension system  
* Added Monitor Switching Support  
* New C++ Widget Framework Added  
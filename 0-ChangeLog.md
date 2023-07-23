# ChangeLogs

V2.0  

* Completely new system introduced.  
* Instanced Object, Data Oriented, Composition based Settings Framework  
* JSON based Settings  
* GameplayTag and Inheritance based extension system  
* Added Monitor Switching Support  
* New C++ Widget Framework Added  

V2.1  

* Added support for Enhanced Input Rebinding  
* Added Audio Output Switching Support  
* Save game variables automatic support added  
* UX improvements for both C++ and Blueprints  

V2.2  

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

# Getting Started

Universal Options is a modular framework that provides a setting layout and lets the user expand each section of the system to their needs.  

![Image](/Resources/Game/SS_Graphics_UI.JPG)  

___  

## Settings

As mentioned in [Readme](/README.md), all our settings get loaded by the Settings Asset. We recommend checking [the Readme's section on project settings](/README.md#loading-settings). This Settings asset contains the setting configurations for:  

* Graphic Settings  
* Sound Settings  
* Input Settings  
* Game Settings  

Graphic, Sound, and Input settings are established via their respective Data assets for configurations while the Game Settings framework expects the user to extend their classes and define the data and logic as they require. You can find more information about our data assets and default settings [here](/1-UnderstandingAssets.md). These are particularly important for our Game Settings framework, where you can define your saving types and then identify the system speifically.  

![Image](/Resources/Framework/SS_SettingsAsset_Minimzed.JPG)  

For Graphics, Sound, and Input settings, the only potential setup required is at the data asset level configuring the specific data to your needs.  

We operate on a [Gameplay Tag System](/1-HowSettingsWork.md#gameplay-tag-system) to identify each of our respective settings both functionally, at runtime, and for saving.  

Settings have 4 different data sets, each treated as JSON objects:  

* Default Data  
* Loaded Data
* Last Confirmed Data  
* Pending Confirmation Data  

Our Default Data is established via MakeDefaultJSONValues() function, Loaded Data is from the last registry on our save file, Last Confirmed data is either the latest saved at runtime or defaults in case of no saved elements, and Pending Data contains the information that we have modified but not yet applied to the system. You can see more information [here](/1-HowSettingsWork.md).  

Our system is controlled by a Game Instance Subsystem and is available locally everywhere. Only Game Settings can exist on Dedicated Server, in case you wanted to keep some transient data persist through elements. Keep in mind that Universal Options does not replicate any of this data.  

___  

## Working with the system

[This page](/1-WorkingWithTheSettingsSystem.md) has far more insights on the inner workings of the framework, but settings have the following major actions:  

* Set Pending - Synchronizes our own JSON pending data to our variable data. seting last confirmed data to our pending JSON data.  
* Discard Pending - Synchronizes our variable data to the last Confirmed JSON data, setting last confirmed data to Loaded data if save exists, otherwise default JSON data.  
* Reset to Default - Synchronizes our variable data to the default JSON data, also setting last confirmed data to Default data.  

When running these Actions, you also have control over the following actions after the data has been synchronized:  

* Apply - executes the settings' apply function. For example, for graphics settings, this is when the quality, resolution, etc is applied at the engine level. For Input, the mappings get rebound, etc.  
* Save - interacts with our saved system and Writes the Last Confirmed Data to disk.  
* Broadcast - Broadcasts Update activity to all our active listeners. For example, say that in graphic settings I modified the resolution data and the device data. Broadcasting these will let our listeners know about these changes.  

You can use our [Listener component](/1-WorkingWithTheSettingsSystem.md#responding-to-changes) for setting up your listening elements from actors and blueprints.  

___  

## Widgets

![Image](/Resources/Widgets/SS_GlobalWidget_Settings.JPG)  

Besides containing our settings data individually, our Settings asset also contains our Global Widget settings, which controls defines the information for how the settings widget get created. [This page](/3-WorkingWithWidgets.md) goes deeper into the workings and dynamics of it, but generally it is a collection of data assets defining the classes, settings, and display details for how does the widget get managed.  

Our widgets use the [listener system](/1-WorkingWithTheSettingsSystem.md#responding-to-changes)  to self update on things based on the [json paths](/1-HowSettingsWork.md#accessing-and-paths) and convert or fetch the data accordingly.  

Game Settings specifically allow the dynamic injection of Widget settings by JSON Path, so you can populate your widgets by declaring entries in a data asset, everything else gets handled for you!  

![Image](/Resources/Widgets/SS_GameWidget_Settings.JPG)  

You can subclass the widget class `UOWidgetGameRow` to specify the given widget functionality and how that affects our actual Game Settings.  

The main settings widget is `UUOWidgetGlobalSettings`. To get the class defined inside the Global Widget Settings asset that is active, you can either do `UUOUniversalOptions::Get(this)->GetWidgetSettings()->GetWidgetClass()` or the utility function, `UUOUniversalOptions::Get(this)->GetWidgetClass()`.  

![Image](/Resources/Widgets/SS_Graph_GetGlobalWidget.JPG)  

You can also get the widget settings and the widget classes for each type individually.  

![Image](/Resources/Widgets/SS_Graph_GetWidgetSettings.JPG)  

You can choose to dynamically instance it at runtime by running this code or alternatively directly add it to your widget design. Global Widgets don't need much setup at the design level, and self-set up themselves at runtime.  

![Image](/Resources/Widgets/SS_GlobalWidget_SpawnNode.JPG)  

### Modifying Widgets

#### Advanced Copy

If you wish to duplicate the widgets to reduce the setup for deep customization of things, you can advanced-copy the widgets folder. Keep in mind that Advanced Copy does not work for any files that are installed at the engine level, such as Marketplace Plugins. You can move the plugin from EngineDirectory/Plugins/Marketplace/CM_Engine_Options to your ProjetDirectory/Plugins/CM_Engine_Options then run Advanced copy on it.  
> Doing tnis may require your project becoming a C++ project to compile the binaries.  

#### Subclassing

You can also subclass our widgets' C++ classes to fit your specific interaction needs and aesthetics. You can see the Widgets inside `PluginContent/v2Content/Widgets` and look into the setups of each, they'll have comments on what you need to do to have them functional.  

### Setting Widget settings

Once you have your custom widget classes, it's time to apply them to the [Global Widget settings](/3-WorkingWithWidgets.md). You can generate your own specific Widget Settings asset depending on the type (Graphics Widgets, Sound Widgets, Input Widgets, Game Widgets) and outline the structure of your project and how the widgets should instance.  

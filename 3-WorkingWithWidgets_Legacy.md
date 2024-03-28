# Working with Widgets (Legacy)

This page covers widget functionality on Universal Options versions 2.0 to 2.2. In 2.3+ they are still functional, but these widget settings have now been marked as legacy. For the up to date version of this, check [here](/3-WorkingWithWidgets.md).  

## Understanding Widget Settings

![Image](/Resources/Widgets/SS_GlobalWidget_Settings.JPG)  

To make your own Widget Settings asset, click Add -> Miscellaneous -> Data Asset and pick the relevant asset type  

![Image](/Resources/Widgets/SS_AssetPicker_WidgetSettings.JPG)  

### Widget General Settings

Our `UUOWidgetGlobalSettings` class spawns and registers its subelements based on the active Global Widget settings Asset referenced by our [Loaded Settings Asset](/README.md#loading-settings). Before going deeper into this, let's overview what the `UUOGlobalWidgetSettings` asset has in store.  

> NOTE: In v2.2+, you can also control which widget settings get spawned via our [Platform Settings](/2-PlatformSettings.md).  

**Global Settings Widget class** is the type of `UUOWidgetGlobalSettings` class selected to fetch when doing  
![Image](/Resources/Widgets/SS_Graph_GetGlobalWidget.JPG)  

You can also however just add your child widget of `UUOWidgetGlobalSettings` directly into your main menu and it'll work just fine.  

**Section Menu Widget Class** is a button widget that gets generated for us during the initialization process, and the idea behind it is as follows: for each `Widget Settings Asset` that we have in our global settings asset (Graphics, Input, Sounds, Games), we generate two of each: A widget for each of these types, and a button header for it.  

![image](/Resources/Game/SS_Graphics_UI.JPG)  

To show this see the image above, the buttons on the left come from this class, and the graphic widget is made automatically from the widget settings inside the `UUOGraphicWidgetSettings` Asset.  

Next up, we have **System Display Names**. This is used for the section Menu Widget class' text display name as seen in the image above. You can specify the names of each section as well as your active game setting sections.  

### Per Setting Type Widget Settings

Our Graphic Settings Asset contains the main widget class to use for our `UUOWidgetGraphicSettings`,the Text Settings for displaying our Graphic Quality, Types, and Window mode. We also choose the Graphic Type widget and which Graphic Types to generate widgets for. In the future, we'd be looking at adding more options for better modularity.  
![Graphics](/Resources/Widgets/SS_GraphicsWidget_Settings.JPG)  

Our Input Settings asset also contains the main widget class of type `UUOWidgetInputSettings`, and the `UUOWidgetInputRow` class to show for rebinding options. This will be even more important when Enhanced input rebinding support is added soon.  
You also get settings for Messages such as binding key open as well as failed messages.  
![Sound](/Resources/Widgets/SS_InputWidget_Settings.JPG)  

Sound Settings for now are quite simple, they let you choose the `UUOWidgetSoundSettings` class and `UUOWidgetProfileSettings` class for showing each profile to display. At the moment the spawning options are the `WidgetSoundSettings` widget. We'll be wanting to move more options to this asset and further customization but that will arrive once audio switching support is added soon.  

Game Settings Widget require a bit more understanding, so let's go through a couple of things:  

You select which Game Setting Assets to be present by the Tag to Game Widget Settings Map. This Tag will represent the game settings that it is able to query.  
![game settings](/Resources/Widgets/SS_GameWidget_Settings.JPG)  

You select the main Widget class by `UUOWidgetGameSettings` to act as your processor and then proceed to choose the given `JSON` paths. For example for our `V1 Settings` included as test we have the `boolean` variable `bIsPermadeath` and a `vector2D` variable `MouseMultipliers`.  

For the widget part,we utilize the `UUOWidgetGameRow` to populate things. In this example we'll use the `bool` widget child to showcase how it can work with [Set Data](/WorkingWithTheSettingsSystem.md#writing-data), and a specialized widget to work on our Mouse Multiplier variable.  After setting the class we can also choose the Display Name, which we'll show how to wire it up.  

### Game Row Widget  

You can configure the name from  
![Image](/Resources/Widgets/SS_Graph_GameSettings_SetName.JPG)  

We can respond to those specific events with `On Settings Update`  
![Image](/Resources/Widgets/SS_Graph_BoolRead.JPG)  

To write we can either use the `SetData()` functionality  
![Image](/Resources/Widgets/SS_Graph_BoolWrite.JPG)  

or for specialized widget settings like the Mouse Multiplier one, we can communicate with them directly.  
![image](/Resources/Widgets/SS_Graph_GameSettings_MouseXY.JPG)  

so that it can look like:  
![game settings](/Resources/Game/SS_Game_UI.JPG)  
___  

## Game Settings Widget Base

Like the [Listener Component](/WorkingWithTheSettingsSystem.md#settings-listener-component), we [respond to both paths and any update event](WorkingWithTheSettingsSystem.md#responding-to-changes). Examples include our Graphics Settings widget, Sound Widget, and even the Game Row Widgets themselves.  

![Graphic Widget defaults](/Resources/Widgets/SS_GraphicsWidget_Defaults.JPG)  

You can override different events for your different needs.  
![Events](/Resources/Widgets/SS_WidgetSettings_events.JPG)  

For example Widget Graphic Type table creates the individual graphics type widgets on initialize and updates them on settings update if it matches `$.graphicProfiles`.  

Widget game row utilizes Set Litening Values for processing the dynamic binding.  

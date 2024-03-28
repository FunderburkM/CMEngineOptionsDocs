# Working with Widgets

This refers to the system being used by v2.3 and above. For the Previous Widget System now marked as legacy, please check [this page](/3-WorkingWithWidgets_Legacy.md).  

## Understanding Widget Settings

![Image](/Resources/Widgets/SS_GlobalWidget_Settings_v23.jpg)  


The global Settings widget class are for widgets implementing the `UOWidgetGlobalInterface` interface and is what is used to generate the entire widget. You can create it from anywhere using the `UUOWidgetHelper::CreateGlobalWidgetFromUniversalOptions` function.    

## Spawning the Widget

![Image](/Resources/Widgets/SS_Graph_CreateGlobalWidget.jpg)  

## Setting Asset Values

This overviews the basic aspects of making your widgets in a data driven way. Please read the sections below for information on how the system actually works and how to implement it yourself if you want to register custom widgets into it.  

![Image](/Resources/Widgets/SS_Builder_GraphicSettingsExample.jpg)  


Each Widget asset can be considered a widget panel in the global widget (explained below).  

Important Information:  

* Widget system tags controls with settings are involved in this widget panel. This means that you can now mix and match setting widgets from multiple systems into a single widget!  
* Widgets created by this system get added into a widget implementing the [UOWidgetContainerInterface](#uowidgetcontainerinterface) interface.  
* Widgets can be wrapped into containers via the [UOWidgetSettingNameInterface](#uowidgetsettingnameinterface) to provide a friendly name. This lets keep widgets modular by only having to create the widgets for things like the checkboxes/string boxes/sliders and avoiding having to recreate specializations over and over just to change a name and the value it is pointing at.  
* Each Array Index in `WidgetBuilderEntries` creates a widget based on the instanced settings and get added to the widget with the container interface.   


## UO Widget Interfaces

Before explaining how the widget builder system itself works, let's first go over the interfaces that widgets using this system need to use. Here are the important ones for this system:  

### UOWidgetInterface

This interface is for setting up validation queries and notification of creation. Creating widgets with our system lets you set up automatic binding settings for hiding/disabling based on function look ups.  

![image](/Resources/Widgets/SS_Builder_Validation.jpg)  

In this image above, we're telling the widget of `W_UO_GraphicsDevice` to only show up enabled if the functionality setting in our Graphics Settings class returns with `IsAllowedtoDisplayOrManageGraphicsDevice` function as true. More information on this system explained below.  

This is controlled with three functions:  

`UOWidgetInterface::SetupSettingValidation`  - This interface function for widgets implementing the interface registers the value set from the data asset setting (see image above) to the widget for later querying.  

`UOWidgetInterface::GetSettingValidation` - This interface function is for querying that value. 

> **Important:** Check the section below on [Widget Validation Binding Condition](#widget-binding-condition).

Via the builder system, you may have widgets wrapped in what we call `Setting Name Widgets`. Whenever the Handle Setting Verification returns false, we want to apply the action on the wrapper widget, not the widget wrapped within as we want to hire the row widget itself, not just the button or the slide that represents the setting.  

![Image](/Resources/Widgets/SS_Builder_UpdateVerification.jpg)  

Several core widget classes that we provide already provide this, such as our base class for the widget system settings `UOWidgetSimpleSettingsBase`. You can find example children classes of this in the `plugin content / V2 / Widgets / SimpleWidgets` and `plugin content / v2 / Widgets / Graphics`.  However, you can extend support to this by just registering the interface to your widget types, both C++ and BP!  

### UOWidgetSettingNameInterface

Default Included Implementation of this interface: `W_UO_Template_Slot` 

This is a simple interface implementation used for `Setting Name Widgets`.  For example if you see this image:  

![image](/Resources/Game/SS_Graphics_UI.JPG)  

You'll see that things like Resolution, Window Mode, Frame Rate Limit, etc are all using setting wrapper names.  

![image](/Resources/Widgets/SS_Builder_GraphicSettingsExample.jpg)  

You see how in this image, you set the setting name widget class, which accepts any widget with this interface and then in the widget builder (more below)  you can specify whether you want to use a name widget and the values for it.  

Important Functions:  

* `UOWidgetSettingNameInterface::SetupValues` This registers the data applied from the builder.  
* `UOWidgetSettingNameInterface::RegisterValueWidget` This registers the widget that this widget will be wrapping.  


### UOWidgetSettingValueInterface

Default Included Implementation of this interface: `UOWidgetSimpleSettingsBase`

This is the right side of the image  the graphics example. The Slider Widget, the Switch Widgets for VSync, etc.  

![image](/Resources/Game/SS_Graphics_UI.JPG)  

Let's disect this image once more:  

![image](/Resources/Widgets/SS_Builder_GraphicSettingsExample.jpg)  

Putting the Widget builder settings aside (this is covered below), you see that in this entry, we set up the `Setting Name Widget` enabled and text name, and then we set up the following aspects:  

* Settings Tag: Which setting does this widget pull data from  
* Setting Name: What is the name of the setting that we're pulling from the Setting we get from the tag. **Important**: This must include the `$.` root. For more information on this, check [Working with Settings](/1-WorkingWithTheSettingsSystem.md)  and [Working with Json](/1-WorkingWithJson.md)  
* Range Gameplay Tag: Optional setting for settings that support it, such as Graphics::FOV, Graphics::ResolutionScale, Audio::Volume, etc. See [this page](/3-ExtendingYourOwnGameSettings.md#ranges) for more information.   
* Widget class: Widget Implementing this interface to be instanced and apply the functions.  

Important functions

* `UOWidgetSettingValueInterface::SetupSettingValue` - this is used to move the information defined above to the widget itse.f   
* `UOWidgetSettingValueInterface::GetListenerBindingValue` - this is used for whenever we run the `UUOUniversalOptionsHelper::RegisterSettingListener` to listen for setting changes on this widget.  

### UOWidgetContainerInterface

Default Included Implementation of this interface: `W_UO_GenericContainer` 

This widget is used by each widget settings asset to hold the widgets created by the widget builder entries (more info below). Its job is to register and unregister widgets created from the builder entries.  

Important Functions:  

* `W_UO_GenericContainer::AddCreatedWidget`  
* `W_UO_GenericContainer::RemoveCreatedWidget`  
* `W_UO_GenericContainer::InitializeWidgetContainer` - We'll want to store the widget settings asset that created us for the getter function right below 
* `W_UO_GenericContainer::GetInitializedWidgetAsset` - Returns the asset that initialized this widget   
* `W_UO_GenericContainer::OnWidgetFinishedConstruction` - Runs after all creation widgets with valid settings have been finished.   

### UOWidgetGlobalInterface

Default Included Implementation of this Interface: `W_UO_Simple_GlobalWidget`  

This widget is referenced by the Global Widget settings and it's used to store the Container Widgets created by the multiple panels to create.  

Important Functions:  

* `UOWidgetGlobalInterface::AddCreatedWidgetContainer`  
* `UOWidgetGlobalInterface::RemoveCreatedWidgetContainer`  
* `UOWidgetGlobalInterface::DeinitialieWidgetGlobal`  

## Widget Builder System

This section covers the Widget Builder System. First, the basics:    

* The base accepted class for this is `UUOWidgetSettingsAssetBase`. The Plugin has specializations for Sound, Graphics, Input, and Game widget setting assets to fit the additional settings with each type of setting.  
* The base class for building widgets is `UUOWidgetEntryBase`. The Plugin implements multiple specializations for this, including:  
    * Create Widget, Create Spacer, Create Image, Create Setting Bound Widget, and more. Developers can create their own Widget Entry Base child classes (C++ Only, BP extension support will come later) if they desire custom widget creation logic.  
* The widgets are build via the `UUOWidgetHelper` Function library. This library includes and handles the vast majority of the functionality responsible for building widgets in the entire plugin.  
* It is the children of `UUOWidgetEntryBase` that utilize the specializations of the widget interfaces and handle their initialization.  

### Widget Binding Condition


This uses the `UOWidgetInterface` for setting up validation queries and notification of creation. Creating widgets with our system lets you set up automatic binding settings for hiding/disabling based on function look ups.  

![image](/Resources/Widgets/SS_Builder_Validation.jpg)  

C++ Implementation example  

```cpp
void UUOWidgetSimpleSettingsBase::SetupSettingValidation_Implementation(const FUOWidgetBindingStruct& WidgetValidation)
{
	SettingsBindingStruct = WidgetValidation;
}

FUOWidgetBindingStruct UUOWidgetSimpleSettingsBase::GetSettingValidation_Implementation() const
{
	return SettingsBindingStruct;
}
```

`UUOWidgetHelper::HandleWidgetSettingUpdateVerification` - This helper function is what executes `GetSettingValidation`, and allows an optional widget to apply the action to. Here's why this matters:  

Via the builder system, you may have widgets wrapped in what we call `Setting Name Widgets`. Whenever the Handle Setting Verification returns false, we want to apply the action on the wrapper widget, not the widget wrapped within as we want to hire the row widget itself, not just the button or the slide that represents the setting.  

![Image](/Resources/Widgets/SS_Builder_UpdateVerification.jpg)  

Whenever the function returns false (in the example above, that either the grpahic settings or the platform specify to not show the graphics device), we apply the Widget Actions On Validation Failure, or Widget Actions on Validation if it returns true. Hiding the widget, disabling it, showing it, etc.  

### Widget Setting Name

Implementing `UOWidgetSettingNameInterface`, these wrap widgets created by the `UUOWidgetEntryBase` types. To be precise, they create them here:  

```cpp
/** Is in charge of just creating the new widget object. Please wait to make any post actions on it (like initializing interface calls) until you reach PostWidgetCreation()*/
virtual UWidget* CreateNewWidget(UUserWidget* OwningWidgetObject) const;

/** Runs after CreateNewWidget and before PostWidgetCreation
* Lets you inject widget actions into the mix. For example, the widget that was created gets wrapped inside another widget, which is what this function returns in this case
*/
virtual UWidget* PrepareWidgetForAddition(UUserWidget* OwningWidgetObject, UWidget* CreatedWidget, UOWidgetSettingsAssetBase* FromSettingsAsset) const;
```

in `PrepareWidgetForAddition` which then things like the widget binding condition can manipulate via the `Widget To Apply Action To`. Keep in mind that `IUOWidgetInterface`, in its `OnWidgetCreation` method, gives the created widget, not the settings name, the optional wrapper widget that contains it.  
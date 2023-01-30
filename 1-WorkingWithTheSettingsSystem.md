# Working with the Settings System

## Getting our Types

You can get our settings either by their Static functions `UUOSettingsType::Get()` or by the Universal Options Subsystem `UUOUniversalOptions::Get()` getters.  
![General Getters](Resources/Framework/SS_Graph_StaticGetters.JPG)  

For example in C++  

```cpp
if (auto* UO = UUOUniversalOptions::Get(this))
{
    if (auto* StandardRebind = UO->GetSettings<UUORebindStandardInput>(UO_Settings_Input))
    {
        //Do something with our Standard rebinding settings. 
    }
    //You can also do UO->GetInputSettings() and then cast, more UX options coming in future releases.
}

//another example

if (auto* Graphics = UUOGraphicsSettings::Get(this))
{
    //do something with our graphic settings
}
```  

---  

## Understanding the Settings System  

At the Subsystem level, there are several getters for given setting types (as shown above) as well as widget options. For interacting with the Saving and applicating settings elements. For interacting with the save system, these are the main elements:  
![Subsystem Actions](Resources/Framework/SS_Graph_SubsystemActions.JPG)  

These actions are also available at the per-setting level  
![Settings Actions](Resources/Framework/SS_Graph_SettingsActions.JPG)  

* **System Action** is in charge or Writing to the System (more below).  
* **Clear Save Flags** will not modify the applied runtime settings, but it'll delete the items in the save game level. For example, say that you have loaded custom settings, and want to delete the save file without resetting the runtime values currently applied (where they'll be applied either next time we load the game or until the next system action).  
* **Is Feature Enabled**  checks whether a given system is allowed to run. This is controlled by both our `Initiation Settings` and `Load System` specifics (more below), which act on them accordingly.  
* **Has Pending Changes** checks whether a given setting has modified settings that have not been applied or saved (more below).  

### Working with Tags

These concepts are used throughout both the subsystem and setting actions, so let's go over them with this example:  
![System Action](Resources/Framework/SS_Graph_SystemAction.JPG)  

* **Systems To Check** is `Gameplay Tag Container` that lets us choose the multiple systems that we want to interact with.  
  * Note: if you wish to interact with a specific Game System, you may chose that Tag specifically, say `UniversalOptions.Settings.Game.Test`. If you with this action to apply to all Game Settings present, choose `UniversalOptions.Settings.Game`.  
* **System Action** is a `Gameplay Tag` establishes what function will get run. This will trigger the nodes that you save above on a per-setting level.  
* **Event** is a `Gameplay Tag Container` that lets you control what type of function should be considered here. You can `APPLY`, `SAVE`, and `Broadcast` (more on broadcast in `Working with Settings` and `Responding to Changes`).  

---  

## Working with Settings

Going off from what's above and [our overview on serialization](/HowSettingsWork.md#saving-and-serialization), let's explain how to work with settings for both our classes and your custom game settings. We'll start by explaining our implementation of V1 Game Settings Blueprint. This class has a `Vector2D` property called `MouseMultipliers`, and `JSON-wise` we treat it as `$.mouseMultipliers`.  

We can modify `MouseMultipliers` in BP however and from wherever we like, however, we need an additional bit of logic for our settings system to register those changes. Let's take a look. We have `SetMouseX()` and `SetMouseY()` functions, which our [widget settings](/WorkingWithWidgets.md) interacts with.  
![Mouse X](/Resources/Framework/SS_V1Game_SetMouseX.JPG)  
We check for differences in the Vector Component, and if it's different, we mutate `MouseMultipliers`. Then, we have the option of running two things:  

* `AddPendingBroadcast()` - this controls the Broadcast List (more about that in a bit)  
* `SetPendingChanges()` - this sets that there are pending changes in our system and run `SynchronizeData(EUODataOperation::SynchronizePending)`.  

This way, next time we run `SetPendingChanges` with an acting event (check the `Event` description above), the system will know there are pending actions to synchronize and then apply, save, and/or broadcast.  

Onto how broadcasts work, we have two ways of broadcasting changes:  

* Group Broadcasting. By Running `AddPendingBroadcast()` like in the image, we are saying that `$.mouseMultipliers` should get added to our Broadcast list, so that next time that we run the Broadcast action, it'll be included for our listeners to respond (check [Responding to Changes](#responding-to-changes)).  
* Single (or immediate) Broadcasting. By Running `AddPendingBroadcast("$.mouseMultipliers", UO_Action_Broadcast_Single)`, we are saying that we do not want to add `$.mouseMultipliers` to the Broadcast List and instead broadcast it to our listeners right now.  

![Single Broadcast](/Resources/Framework/SS_V1Game_SetMouseX-SingleBroadcast.JPG)  

> A good example of Single Broadcasting can be found in our Graphics Widget when we switch profiles. You'll see that when you switch from Global `Low` to `Medium`, all the individual graphic types will switch accordingly even though we have not applied, saved, nor group-broadcasted anything (as in Graphics Settings we also have things like `$.resolutionData`, `$.deviceData`, `$.screenData`, and we don't want those to get broadcasted right now, only our specific type).  

---  

## Accessing Data

### Working with Paths

The basics of `JSON Paths` is overviewed in the [How Settings Work](/HowSettingsWork.md#accessing-and-paths) section. We utilize `FUOJsonPath` for this functionality, which can analyze any json object (C++ level functionality). Our settings use this for reading, writing, and listening.  

#### Reading Data

 `UUOSettingsBase::GetData`  
![Get Data](Resources/Framework/SS_Graph_GetData.JPG)  

Here's an example for how you can use this: Say that you want to access the Current Graphics Active Profile's Name.  
![Get Data Graphics](/Resources/Framework/SS_Graph_GetData_Graphics.JPG)  

How you decide to work with these is up to you, this just showcasing how you can operate with them.  

#### Writing Data

`UUOSettingsBase::SetData()`.  

`SetData()`'s support is up to the specific Setting Class' `UUOSettingsBase::ParseSetData` implementation.  

> **Note**: Our base C++ classes don't implement this, so we encourage to use specialized functions (for example, `UUOGraphicsSettings::SetGamma()`) when available.  
> **Note**:  V2.1 now provides automatic UPROPERTY with SaveGame tagging to be supported by SetData without overriding.  

We include an example for working with this inside our V1 Test Game settings:  
![Parse Set Data](/Resources/Framework/SS_V1Game_ParseData.JPG)  

So doing the following will be able to modify our `IsPermaDeath` variable, and in this case apply, save, and broadcast that specific setting.  
![Set Data](/Resources/Framework/SS_V1Game_SetData.JPG)  

### Responding to changes

There are two main ways to keep up with the system's changes.  

1) By subscribing to our System's OnUpdated delegate and checking its tag type. For example, here we're filtering to respond to the graphics' settings.  
![Delegate](/Resources/Framework/SS_Graph_SubsystemUpdate.JPG)  

2) By path, using the Listener Framework. Any object implementing `IUOSettingsUpdateInterface` can subscribe and respond to these events, and use the `RegisterListener()` function for the settings themselves.  
![Register Listener](/Resources/Framework/SS_Graph_ListenerUpdate.JPG)  

`IUOSettingsUpdateInterface` is a C++-only interface. Our Widgets utilize this to automatically respond to this ([Check Widget Settings](/WorkingWithWidgets.md)). You can most easily respond to messages using our `UOSettingsListenerComponent`.  
> You can also use the component as a reference if you wish to do custom interface implementations on uobjects.  

#### Settings Listener Component

This Actor Component is a `Modular Gameplay Component`, so you can add it via Game Features.  

You can see it in use in our Test Content for the Pawn setup. Here we listen to the setting `UniversalOptions.Settings.Game.V1Compatible` and use `Subscribe To Specific Json Paths` (Listen Method #2), searching for the setting `$.mouseMultipliers`. You can see in the test level how this affects our Test Pawn's Rotating Rate.  
![Listen component Settings](/Resources/Framework/SS_ListenComponent_Settings.JPG)  
![Listen Component](/Resources/Framework/SS_ListenComponent_Example.JPG)  

You can also subscribe to `All Activity Delegate` (Listen Method #1).  
![All Delegate Update](/Resources/Framework/SS_ListenComponent_Example-2.JPG)  

### Working with JSON

Refer to [Working with JSON](/1-WorkingWithJson.md) for more information on C++ and Blueprint workflow.  
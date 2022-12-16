# How Settings Work

When opening our `UOSettings_Defaults` Asset, we get greeted with the following:  
![Settings Asset](Resources/Framework/SS_SettingsAsset_Minimzed.JPG)  

Before diving into these specifically, let's go over the general layout of the framework.  

* The Framework is controlled by the `Game Instance Subsystem` class `UUOUniversalOptions`.  
* Each Setting type is a class of type `UUOSettingsBase` and they are kept alive by the system.  
* We load these Objects via our `UUOSettingsAsset` as Instanced Objects, allowing us to set both the specific class of them as well as their settings at the data level.  
* Settings are handled as `JSON` objects. Each settings class gets loaded from and serialized to a `JSON` object.  
* We organize our settings via `Gameplay Tags` of structure `UniversalOptions.Settings`  

> Note: For those unfamiliar with the concept, you can check [this wikipedia link](https://en.wikipedia.org/wiki/JSON) on `JSON` basics.  

## Gameplay Tag System

The plugin comes with the following tags:  
![Tags](Resources/Basics/SS_ProjSettings_Tags.JPG)  

### Setting Types

* We have the following types of settings:  
  * Graphic Settings `UniversalOptions.Settings.Graphics`  
  * Sound Settings  `UniversalOptions.Settings.Sound`  
  * Input Settings `UniversalOptions.Settings.Input`
  * Game Settings `UniversalOptions.Settings.Game`  

For access in C++, check `Sources/UniversalOptions/(Public or Private)/Defaults/UONativeTags`, for example `UO_Settings_Graphics`.  

You can expand this system by creating new tags and applying them to your given objects. This is especially important for our Game Settings, which we'll explore deeper further.  
![Tags](Resources/Basics/SS_ProjSettings_Tags-2.JPG)  

For the Other tags like `UniversalOptions.Action`, check the link below.  

---  

**For a more comprehensive understanding of some of the concepts that will be mentioned in the rest of this page, we recommend checking [Working with the Settings System](/WorkingwiththeSettingsSystem.md).**  

## Accessing and Paths

We utilize the concept of `JSON Paths` throughout our systems. In short, a json path gives us a way to navigate through a `JSON` Object. Let's go with the following example:  

```json
{
    "myFloat" : 42.0,
    "myObject" : {
        "myBool " true
    },
    "myArray" : [ 
        0, 
        24, 
        "somestring",
        {
            "myString" : "hello world"
        }
    ]
}
```

* For accessing "myFloat", `$.myFloat` outputs `42.0`  
* For accessing "myBool", `$.myObject.myBool` outputs `true`  
* for accessing "myArray", `$.myArray` outputs `{ 0, 24, "somestring", { "myString" : "hello world" }}`  
* for accessing "myString", `$.myArray[].myString` outputs `hello world`  

If you wish to play around with this further, you can check this [nice tool](https://jsonpath.com/).  

For our plugin, we use this with `FUOJsonPath`. More specifics in [Working with Paths](/WorkingWithTheSettingsSystem.md#working-with-paths).  

## Saving and Serialization

We can serialize any type of data that is not a UObject. Primitive types, strings, enums, structs are welcome. You can use a `Builder` programming pattern for serializing UObject data into structs and back from structs, but more on this later.  

However, Settings being `JSON` Objects is only needed at three stages: Setting Defaults, Loading, and Saving! For the rest of things, you can work with your regular types. All that matters is that we load to and from `JSON`. To explain this, let's look at our `Test` Game Settings.  
![Test Content Folder](Resources/Framework/SS_ContentBrowser_Test-Content.JPG)  

Here, we are going to be demonstrating working with a custom struct, `S_UO_HeroSettings`.  
![Default](Resources/Framework/SS_TestGame_Defaults.JPG)  

In `MakeDefaultValuesToJson`:  
![MakeDefault](Resources/Framework/SS_TestGame_MakeDefaults.JPG)  

We feed it to the object that will be treated as default. **This is important for whenever we `RESET` our settings, as whatever gets stored in this object is what is treated as our defaults.**  

In `SynchronizeData`, we control both Writing to the `JSON` object whenever we store Pending Changes which can be saved, or read from the source depending on the action.  
![Synchronize](Resources/Framework/SS_TestGame_Synchronize.JPG)  

We have several Data Operation options:  
![Data Operation](Resources/Framework/SS_Graph_OperationData.JPG)  

* **Synchronize Pending** is where we want to write to the `JSON`. Say our game modifies our `Available Heroes` Array variable through different points, and we run `Set Pending Changes`. Then `Synchronize Data` runs with `Synchronize Pending` where we can let our `JSON` object catch up.  
* **Revert Pending** is for reading from the `JSON` to undo pending changes. In the example above where the game modified `Available Heroes` but hasn't saved, we then rollback to the last loaded by the system, which can be either `Saved` or `Defaults`.  
* **Set To Default** is for whenever we `RESET` our given settings object, which we then read from what we defined at `MakeDefaultsValuesToJson`.  
* **Load From JSON** Lastly, this runs whenever the data is being loaded from our save file.  

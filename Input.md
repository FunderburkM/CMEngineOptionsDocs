# Input

#### Table of Contents  
[Base Description in 1.0](#base-description)  
[Implementation](#implementation)  
[Warnings](#warnings)  
[Updates Section](#updates)  
-  [Option Updates & Cancellation Keys](#option-updates)    
-  [Compound Keys and Manual Rebinding](#compound-keys--manual-binding)    
-  [Input Profile Data Assets](#input-profile-data-assets)  
-  [Input Binding Text Collection Data Assets](#input-binding-text-collection-data-assets)  
-  [Input Key Layout Data Assets](#input-key-layout-data-assets)  
-  [Controller Query Detection](#controller-query-detection)  
-  [Technical](#technical)

For Information on Input Settings related widgets (1.1+), please [check this page](WidgetSetup.md#input-widget).  
For Information on saving and loading configuration, please check our [saving system](Framework.md#save).  
For Information on the Redirect System, please check our [Redirect page](Framework.md#redirects).  

Handle your input like never before!   

#  Base Description  
> Initial Base Description on 1.0 (check below for updated states in 1.1+)

- You can have as many Input Profiles as you want!   
- Have profiles only accept specific inputs [Input Profile Type; Keyboard, Gamepad, or All]  
- You can allow/block Axis and Actions to exchange places. [Allow Interbindings]  
    This means that currently bound Action Keys can switch Keys with currently bound Axis Keys.  
- You can have Profiles fill themselves with whatever information you have from Config by Using bAutofill. Alternatively, you can make, from the config Panel, the entire mapping for a given profile! [Exception is if you save a given Input Profile, then it will load that instead]  

> For More information, check [Implementation](#implementation)  

Input Profiles save individually, and the Editor always reloads the bindings that it had prior to Game Launch on Game End. Keys don’t get rebuilt until Settings are Applied/Saved.  

### Input (NON INTRUSIVE)
Input Rebinding is handled through the C++ class [IInputProcessor](https://docs.unrealengine.com/en-US/API/Runtime/Slate/Framework/Application/IInputProcessor/index.html). This means that when the gate is active, it will intake the input before any Player Input Component, preventing potentially unwanted clicks - it also means that you do not need to implement any custom pawns, player controllers, nor input components to get rebind keys! Keys that are set as Static also close the gates. By Default, these keys are Escape and Back. 

### Test Setup 
> For the Test Setup,
Profile 1 -> Keyboard Only (AutoFill) (Don't Show Static)
Profile 2 -> Gamepad Only (AutoFill) (Don't Show Static)
Profile 3 -> All (AutoFill) (Show Static, Disabled)
Profile 4 -> All (Custom Keys) (Show Static)
AutoFill means that on App Startup, it will fill the values with the input from Engine/Input Config
Each Profile can be customized as you want it  and are not hard coded to this test setup.

## Implementation 

To set up your Settings, go to Project Settings -> Plugins Section -> CM_Engine_Options and scroll to the Input Settings Section.  

![image](https://user-images.githubusercontent.com/28312571/147319514-87ce2e77-eb5b-4367-8eac-8f7edebb9aae.png)  

Let's start with the overall structure  
![image](https://user-images.githubusercontent.com/28312571/158043374-f4ba81c0-5005-4007-8300-3eb19cedcf41.png)  

- `CurrentProfile` We identify our profiles by Name. Each Profile will have a given Name configuration. We use this name to identify the element across all our systems. This includes `Current Profile`, which keeps track of the name of our current configuration.   
- `MapAlternateNames` are for inputs that can be considered negative. This is almost always exclusively for Input Elements. For Instance, say that you have the Input Axis Binding named "MoveForward". In traditional setups, you will have MoveForward with a +1.f and a -1.f value, each mapped to a different key. With this map, we can specify how we want the negative version of these elements to be recognized.  
- `Profiles` Holds each of the configurations in their entirety.   

Moving on, An Input Profile  
![image](https://user-images.githubusercontent.com/28312571/158043448-0a3ffdce-bd62-41e7-aca5-f5792d8e1e09.png)  

- `ProfileName` is their Configuration that they're identified by  
- `VisualConfigurationName` is used for linking [Key Layout Data Assets](#input-key-layout-data-assets), which are used for establishing which Keys are accepted for this current profile, as well as their visual configuration.  `1.1+`  
- `ActionsSaved` holds the ActionMappings to be considered by this profile  
- `AxisSaved` holds the AxisMappings to be considered by this profile
- `InputNamestoNeverShow` is reserved for front end elements (widgets); which mappings (either action or axis) do we want to never show. This is separate from Static Keys.  
- `StaticKeys` what are the names of either ActionMappings or AxisMapping that cannot be rebound. Trying to rebind these will result in a fail and for auto mapping, it will cancel the request.  
- `RebindCancellationKeys` which keys (keyboard/gamepad/etc) will cancel the request for auto mapping. More information [here](#option-updates).  `1.1+`
- `InputRebindScanType` How do we handle conflicts. For Instance, say that we want to rebind our Interact Key "E" to "F", but "F" is already bound to Melee.  
- `InputProfileType` which keys do we accept from devices (Keyboard and Mouse vs Gamepad)  
- `bAllowInterBindingsModel` as an extension of Rebind Scan Type, which types of replacements are allowed. Say for instance that we don't want  Action Mappings to be able to exchange values with Axis Mappings. You configure this here.  
- `bShowStatic` Mostly used for widgets, how do we try to parse bindings that are found in Static Keys.  
- `bLogResults` Whether we showcase most of our result keys on the Output Log.  
- `bAutoFill` when True, we will try auto filling our `ActionSaved` and `AxisSaved` to the values in the Input configuration file. Otherwise, we'll respect the values set at default/data table level.  

An example for a profile that Shows Statics as Disabled (image b) vs one that doesn’t show them (image a) 

![image](https://user-images.githubusercontent.com/28312571/147319722-ae08706f-6f6f-4d50-9ce4-d87136630686.png)
> Widget Image a). Widget version 1.0   

![image](https://user-images.githubusercontent.com/28312571/147319843-2f7b2a99-5feb-4371-8e78-157d36b1e05d.png)
> Widget image b). Widget version 1.0  

### Listening to Events

We can listen to overall major events from the Subsystem such as the following:  
![image](https://user-images.githubusercontent.com/28312571/158044004-216b4304-ace0-4e1d-b921-a4fc48a05d6d.png)

For more information on Functions, please check [Technical](#technical).  

# Warnings 

For Actual Apps, don't use Apply Settings for Input, as that might cause issues [where the System & Save have congruent data but the currently built keymaps don't match that]. For applications, stick to Save Functions [Settings_Apply_PendingSave(true) ].

***

# Updates

![image](https://user-images.githubusercontent.com/28312571/147319884-919b2cd6-1a46-42d0-bd46-a4cb7dc63c75.png)
> Input Widget page in 1.1  

## Option-Updates

> Extended Switch options & Cancellation Keys & Detection Methods is available in 1.1  

**Switch Options**   
In 1.0, you can only either allow inter binding switching at all levels or none. Now, we are allowed to choose between them. Switching at all levels [axis with actions], only switching internally [eg: only axis with axis], or none [cannot switch if binding with exists already.  
![image](https://user-images.githubusercontent.com/28312571/147321305-4e48b3cc-d7f0-45eb-abcf-33fa861e3606.png)

**Cancellation Keys**  
In 1.0, the only way to prevent a key to be recognized for switching was static keys, which only look for the actual input action and axis mapping. However, there was no way of having specific keys always act as binding process cancellations. If you had “Escape” as a static key, but you didn’t have an action mapping called “Escape”, you could not cancel it with the escape key. Now, you can. Additionally, you can also set up specific layouts for accepted keys. Check Input Key Layout Data Assets.
![image](https://user-images.githubusercontent.com/28312571/147321355-0ebbbbc1-c765-4f9b-b4a6-ba2bc031a533.png)

**Detection Models**  
In 1.0, we only allowed the switching of keyboards and mouse events when the mouse buttons did not interact with a widget focus. In 1.1, we detect Mouse, Gamepad, and Keyboard.

## Compound Keys & Manual Binding

> Available in 1.1  

In 1.0, you can only rebind inputs through direct device input, which disallows for key rebinding with modifiers, such as LCtrl, LShift, Alt, and Cmd. In 1.1, you can now Manually rebind Bindings, providing heaps more functionality and usability for both games and production/visualization applications. For the Widget implementation of this, check Widget Setup.

![image](https://user-images.githubusercontent.com/28312571/147321517-c149871e-671a-4302-a074-7d03313c4633.png)  

We refer to this as `Manual Binding`, and 1.0 / Standard Gate Rebinding as `Auto Binding`. 

## Input Profile Data Assets

> Available in 1.1  

In 1.0, you can only set up profiles directly at the config level, making testing entirely different layouts beyond just different profiles a laborious experience. Now, you can specify these settings at a data asset level, which means that you can switch entire configurations easily.   
Note: These settings will get applied to the default layouts whenever there’s a valid data asset pointer. If there isn’t, we’ll just load the values specified at the config level for Input Component.  

The structure follows the same principle as the Settings at the Plugin Level.  

![image](https://user-images.githubusercontent.com/28312571/147321595-f5336ff7-1b88-48a2-afaf-751d2c2f8e91.png)


## Input Binding Text Collection Data Assets  

> Available in 1.1  

An annoying thing about 1.0 is that the default implementation displayed quite ugly results when it came to the input names, as it displayed the input binding name directly. This Data asset provides Text [localizable] configurations to give your values. Negative Text Name is almost always only used only on Input Mappings, where `Move Forward` was a +1 and a -1 set of keys, each bound to a separate register.    

Our Widgets utilize these Values when possible to display Input Row Names.   

![image](https://user-images.githubusercontent.com/28312571/147321671-5bda33cd-c470-4b0f-add5-2cb23f777d99.png)

![image](https://user-images.githubusercontent.com/28312571/147321728-8eae43c9-6c1a-458b-812a-0aa367f2656b.png)

![image](https://user-images.githubusercontent.com/28312571/147321708-a5de4c9a-913f-4e8d-ba1a-e1de3d622129.png)

For quick setups, we have included the Blutility ‘BPEW_SetInputBindTexts’, which will try to auto fill it from your current input settings inside Project/Input

![image](https://user-images.githubusercontent.com/28312571/147321784-3466b305-46e6-4ac6-93df-b13105e0d64a.png)

## Input Key Layout Data Assets 

> Available in 1.1  

Also annoying in 1.0, is that showing the values of the keys show the literal names of the Keys at the engine level, which is undesirable. In 1.1, we can now specify a Text [Localizable] and even a display texture, great for example in gamepad configurations or custom UI designs. 
Input Profiles now have a variable called “Visual Configuration Name”, which is used to find a profile in said data asset. For example,

![image](https://user-images.githubusercontent.com/28312571/147321887-1c756204-d7c2-4eff-bbcb-ad1bc3813e5e.png) 

Which respectively has the content in the following image.

![image](https://user-images.githubusercontent.com/28312571/147321922-c5e9e808-2510-40a9-bc2a-b0fd76f0a2a2.png)  

Let's break it down a bit. We hold a collection of Configurations, where:  

- Each is identified using `KeyArrayProfileName`. As mentioned in [Implementation](#implementation), we will link profiles to these configurations by the value of this variable.  
- `KeyCollection` A Map of available Keys that this configuration accepts. Say for instance that we have a Keyboard layout, and we want to limit entries to only letters and numbers, not allowing F1-F12 keys to be bound. We can specify this here by creating a layout in which F1-F12 keys are not included.  
- `KeyCollectionDataAssetSource` if available during the loading values phase, we will set `KeyCollection` from the Elements in this Data Asset.   
- `MainProfileType` determines the type of input that this configuration applies to.  

As another example case for `KeyCollection`, say that you only want a given set of keys within the gamepad world [to fit a specific configuration, like Xbox vs PS vs Driving Analog vs Work Board etc]. 

![image](https://user-images.githubusercontent.com/28312571/147321942-d3199a2c-766c-422e-a6a0-527ec8f60402.png)
> Example of the configuration in the above image in the input widget in 1.1 vs the old display method in 10  

Additionally, for initially filling configurations out we have included an Editor Utility ‘BPEW_SetInputConfigDataLayout”   
![image](https://user-images.githubusercontent.com/28312571/147322027-3fa590f9-6878-457f-9a48-8c80b98d8b3e.png)

## Controller Query Detection

> Available in 1.1 [Windows Only]  

In 1.1, you can now query Active Controllers, fetch, and auto detect when they are plugged in and out.  You can query them/fetch them directly and/or subscribe to events detected by the subsystem. 

> Even on Windows, this is disabled by default. To enable it, you will need to modify the plugin inside   
CM_Engine_Options/Public/LPSS_Engine_Options.h, and change the value of #define DETECTCONTROLLERS 0 to 1.   
We recommend to do this at a project Plugin Level. You can check project Plugins for more information.  

Query Variable and Functions           |    Delegate that gets called when it detects a change
:-------------------------:|:----------------------------------------------------------: 
![image](https://user-images.githubusercontent.com/28312571/147322180-d6557606-4385-4987-88af-5746bcda5266.png) | ![image](https://user-images.githubusercontent.com/28312571/147322202-1d215b8e-ea5f-4d5c-be1b-742a07a1f4e4.png)

***

# Technical 

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md).  

The functions/elements you will likely be referencing/using are the following (Images taken from `/Content/BP_Options_Documentation.uasset`)  

![image](https://user-images.githubusercontent.com/28312571/158044034-59138bee-eab4-4ab3-bae2-37e98fc558db.png)  

![image](https://user-images.githubusercontent.com/28312571/158044062-bbc9079a-0520-44c5-855a-3a6ba80d8113.png)  


Base classes in 1.0  

- **FEngineOptionsInputAction**  
  Exposed version of FInputActionKeyMapping (Action Bindings, such as “Interact”) with rebinding, add, and removal functionality.
- **FEngineOptionsInputAxis:**   
  Exposed version of FInputActionAxisMapping (Axis Bindings, such as “Move Forward”) with rebinding, add, and removal functionality.
- **FEngineOptionsInputChange:**   
  Stores the base information for whenever an Input Gate for rebinding is open.
- **FEngineOptionsInputProfile:**   
  Implements a single Input Profile as a whole. Stores Action and Axis bindings, has the general rules saved, and keeps an Input change as a tracker. Is responsible for actually rebinding and handling the overlapping keys.
- **FEngineOptionsInputComponent:**   
  Implements the Input System in its entirety. Handles in between the Profiles, Default Bindings, Alternate Names, and Currently used Profile. 
- **IIP_Engine_Options**   
  Is the IInputProcessor class that interacts with the Subsystem whenever the keybinding gate is open.


Class extension and updates in 1.1  

- **FEngineOptionsInputLayoutKey**   
  Holds the configuration of a single element: text & texture
- **FEngineOptionsInputLayoutSubCollectionDataAsset**   
  Defines a specific collection of InputLayoutKey structures based on FKeys
- **FEngineOptionsInputLayoutKeyArray**    
  Holds a setup  by name, device filtration, and loading type of key collections; they represent a specific subcollection of keys
- **FEngineOptionsInputBindsTextStruct**   
  Is a single configuration entry for linking up Binding Names to specific text and category configurations
- **UEngineOptionsInputBindsTextDataAsset**   
  Holds a collection of BindsTextStructs and is used as the main source of bind text collections
- **UEngineOptionsInputLayoutCollectionDataAsset**   
  Is a data asset that holds the different SubLayout configurations and is used to fill and link them with the actual input profiles.
- **UEngineOptionsInputProfileConfigDataAsset**   
  Is the swappable data asset that contains an InputProfile, allowing users to quickly setup and swap different configurations. 


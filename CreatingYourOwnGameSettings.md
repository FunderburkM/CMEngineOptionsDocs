# Creating your own game settings

We recommend that you have checked [Understanding Data Assets](/UnderstandingAssets.md)  at at least a little bit of [How Settings Work](/HowSettingsWork.md). That said, let's go!

First, let's create our subclass. We recommend creating a class straight from `UUOGameSettings`, but you can choose to subclass `UUOGameSettingsFromV1` if you wish to continue from the support of the v1 types in case you had v1 save game settings.  


![Image](/Resources/Assets/SS_ClassPicker_GameSettings.JPG)    

With the type created, let's go over a few things. The `why` of this is explained [here](/HowSettingsWork.md#saving-and-serialization), for now let's just do a step by step of what's needed.  

For showcase purporses, we'll duplicate the setup of the test game settings included in the plugin. We'll be testing with a blueprint struct S_UO_Hero, and make an Array Property of this struct.  

![Image](/Resources/Framework/SS_TestGame_Defaults.JPG)    

Now, we'll need to override two basic functions `MakeDefaultValuesToJson` and `SynchronizeData`. Make sure that for both of them, you call Super (C++)/Parent (BP)!  

In MakeDefaultValuesToJson, we add the values that we want to be considered as Defaults, so we'll be setting the field `Heroes` to be our struct array.  

![Image](/Resources/Framework/SS_TestGame_MakeDefaults.JPG)  

In Synchronize Data, we have to do two things: 
  * if Data Operation is `Synchronize Pending`, then we need to write to the input json to update it with our latest Heroes struct array.  
  * else, we need to read from the input json to update our Heroes variable from said Json. This will happen when discarding unsaved changes, resetting to default, or when loading from save file.  

![image](/Resources/Framework/SS_TestGame_Synchronize.JPG)  

Additionally, you can implement the `ParseData` function, which allows you to generically set information to your settings from anywhere. Its dynamics are explained [here](/WorkingWithTheSettingsSystem.md#writing-data).  

![Image](/Resources/Framework/SS_TestGame_SetData.JPG)    
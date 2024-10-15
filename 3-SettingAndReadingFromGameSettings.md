# Setting And Reading From Game Settings

For the content of this page, we recommend taking a look at the following sections:  

- [How Settings Work](/1-HowSettingsWork.md)  
- [Understanding Tags and Actions](/1-WorkingWithTheSettingsSystem.md#understanding-the-settings-system)  
- [Game Settings Overview](/2-GameSettings.md)  
- [Creating your Game Settings](/3-CreatingYourOwnGameSettings.md)  
- [Extending your Game Settings](/3-ExtendingYourOwnGameSettings.md)  (Optional)  

## Getting your settings  

There are two main ways to get your settings and their data  

### Getting The Settings Object  

- By Tag (With or without Class)  
- By Class  

![Image](/Resources/Framework/SS_Graph_StaticGetters.JPG) 

### Getting the Data Directly  

You can read or set its data directly based on our [JSON Path system](/1-WorkingWithJson.md). [This Page](/1-WorkingWithTheSettingsSystem.md#accessing-data) goes over it in detail.  

This can be achieved in multiple ways:  

- Getting the Object type, then Call Get Data. This will return a JsonValue struct that you can use our extensive conversion functions to auto convert to your type. Check the JSON page for details.  
- Running `Get Subsystem Data()`, which bundles the call of getting the Object by Tag, then doing `Get Data()` On it. 


## Setting Data  

Likewise, there are three main ways to do this  

### Getting your object and Writing Directly to your class

This involves getting the game settings game object and casting to your type (you can see Get settings By Class which returns a casted version or do the cast yourself).  

From here, you can get the data that you care about, set it, and then register it for broadcasting and then register an action. 

For example, you could get the Game Settings V1 Object with Tag and Class, and then change Run a function to set its values.   

![Image](/Resources/Framework/SS_TestGame_SetMouseX.jpg)  

Add Pending broadcast works with the path (which will almost always be $.TheVariableName; the cases where it is isn't the case is for when you specify the paths yourself. The core graphics, audio, and input settings do this). 

This Pending is important as it registers the path for broadcasting next time the system broadcasts changes. So in this case, because widgets and potentially pawns could depend on these, we need to make sure that the path is registered so that the depending objects can respond.  [More Information on Broadcast And Set Pending changes here](/1-WorkingWithTheSettingsSystem.md#working-with-settings).  

Then, we set Pending Changes. Like mentioned in [Working with Tags](/1-WorkingWithTheSettingsSystem.md#working-with-tags), we can choose what we can to do when writing this information. We may not want to do anything either, or broadcast the single change, apply, save, etc.  


### Getting Your Object Just by Tag and Writing via Paths 

Using our [JSON Path system](/1-WorkingWithJson.md), You can get a hold of the object by tag or class and use `Set Data()` on it. For example, with game settings v1, you can get the  settings by Tag and then call `SetData("$.bIsPermaDeath", FUOJsonValue(false)`);. Please note that the pathing implementation then depends on the game settings' parsing implementation. [More Information can be found here](/3-CreatingYourOwnGameSettings.md#automated-vs-manual-support-notes).  This same principle applies in Blueprint, just using the ample conversion functions to `FUOJsonValue`.  

### Static Writing by Tag and Writing Via Paths  

Same Idea as above, except skipping one step. [In Depth Overview here](/1-WorkingWithTheSettingsSystem.md#writing-data).  

![Image](/Resources/Framework/SS_Graph_SetData.JPG)  
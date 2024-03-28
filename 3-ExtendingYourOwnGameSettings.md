# Extending Your Game Settings 

Once you have your game settings type created ([Check this page](/3-CreatingYourOwnGameSettings.md)), there's a lot that you can do with them. Besides specifying what gets saved and _how_ (manual vs automatic property saving), you can also extend this system significantly to fit your needs. Keep in mind this applies to all settings in the plugin, so you can extend graphic settings and add the behavior you want/need.  

![image](/Resources/Framework/SS_TestGame_Overrides.jpg)  

You can specify pre loading information, loading confirmation, redirect handlings (such as setting renaming backwards support), init/deinit, beginplay/world init/world deinit.

On the loading and settings side, you can override HasPendingChanges, ExecuteApplySettings (for reflecting the changes on the world, such as applying latest graphical settings or newest input rebindings in game), and even handling custom information for setting json information directly.  

Of course, as any class, you can also extend and make any and all functions and functionality that you may want for your convenience!  

![Image](/Resources/Framework/SS_TestGame_SetMouseX.jpg)  

## Ranges

With the addition of v2.3, you can now specify ranges for your settings! This is great for specifying accepted ranges of values for the system to interact in between, also opting into the renewed [widget system](/3-WorkingWithWidgets.md). 

in v2.3, we start with Integer Ranges and Float Ranges.  

![Ranges](/Resources/Framework/SS_Graph_Ranges.jpg)  

An Example from Graphic Settings:  

```cpp
bool UUOGraphicSettings::GetFloatRangeValue_Implementation(FGameplayTag RangeGameplayTag, FName AdditionalSpecifier, FFloatRange& OutRange) const
{
	if (RangeGameplayTag == UO_Settings_Graphics_Range_Gamma)
	{
		OutRange = ScreenData.GammaRanges;
		return true;
	}
	if (RangeGameplayTag == UO_Settings_Graphics_Range_ResScale)
	{
		OutRange = ScreenData.ResolutionScaleRanges;
		return true;
	}
	if (RangeGameplayTag == UO_Settings_Graphics_Range_FOV)
	{
		OutRange = ScreenData.FieldOfViewRanges;
		return true;
	}
    //rest of the function 
    return false;
}
```

Example of usage:  

![image](/Resources/Framework/SS_Graph_RangeWidgets.jpg)  
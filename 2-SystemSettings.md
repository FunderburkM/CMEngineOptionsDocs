# System Settings

Settings System relating the handling of the UO system itself. Save Game Files, etc. This system will grow substantially in the future.  

In V2.3, this system provides the following two services:  

* Save Files to Delete On Load - this helps the developer clean up any files that they no longer want from either previous game versions, changing systems, etc. It will remove save game slots that match that name, so it is not specific to having to be part of Universal Options.   
* Redirect To Universal Options - this helps the developer deal with changes in the Project Settings -> Save Game File Name. Say that in the past it was UniversalOptions[.sav] and throughout versions you've changed it to MyGame[.sav], MyGameSettings[.sav], etc. This now lets you input all those aspects and try to link them to universal option settings. Please keep in mind that if it does find a matching save game name that Universal Option directly uses, it will delete the old file after redirecting its content to the last updated name in project settings.  
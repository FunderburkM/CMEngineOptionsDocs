# ChangeLogs

# 1.1 

State: Released on 4.25, 4.26, 4.27  

New Features  

- Graphic Auto Detect Options
- Graphic Default Window & Resolution Settings
- Graphic Load/Config Settings & save config options
- Graphic Quality Type and Value Text settings 
- Sound Load/Config Settings
- Game Settings Load/Config Settings
- Input Binding from FKey to FInputChord [allowing bindings with modifiers]
- Input Text Configurations for Action and Axis Bindings
- Input Layout Configurations for different Input Profiles
- Input Sub Layout Configurations for visual Display [Text/Texture]
- Input Profile Data Asset implementations
- Input Switching Cancellation Keys and Switch Profile Options
- Controller detection has been added. [WINDOWS only]
- All of 1.0’s example widgets have been refactored as modular widgets in C++, allowing a whole new level of customization and ease of implementation for your projects’ specific needs. 

Bug Fixes:
- Fixed an issue on graphics switching where resolution changes were not being registered properly if you hit apply and then save changes. 
- Fixed an issue on graphics where field of view interface calls didn’t always reach the desired target.
- Fixed an issue on graphic quality type switching where it would switch out of enumerator bounds.
- Fixed an issue with Input switching where in some cases, switching between an axis mapping and an action mapping would error out regardless of whether the interaction was allowed or not. 
- Fixed an issue with Sound Layers where after getting disconnected forcefully from a server and loading the main menu, sound value modification didn’t work anymore
- Fixed an issue with Sound Layers where sometimes, loading saved values didn’t apply the proper values until applied manually by the user.

# 1.2

State: Released on 4.25, 4.26, 4.27  

New Features  

- Game Settings now has `UEngineOptionsGameSettingsDataAsset`, which allows the user to swap Settings easily - Also available in Runtime!  
- Audio Settings now has `UEngineOptionsSoundDataAsset`, which allows the user to swap Settings easily - Also Available in Runtime!  
- Added a new layer of Widgets! Introducing the Concept of Example Widgets. For 1.2, the Default Sample was released.  
  - With the addition to the new layer of widgets, the generic widgets in both /Test and /V1_1 have been generalized into C++  
  - Example widgets aim to ease in integration by abstracting most of the logic away from the user so you can focus on making them look flashy!  


Improvements:  

- Improved the accuracy of Auto Detect Settings  
- Massive Overhaul of Commenting inside the plugin - Variables and Functions now have much more extensive descriptions. especially for C++ types  
- Added Macros for Logging and Function handling to improve the readibility of the code  
- Added bLogResults functions for Widgets that output log for misconfigurations, which sometimes clouded the output log  

Crash Fixes / Safe Guards:   

- Added safeguards for user misconfigurations such as having no Graphics Data Table set / a Table with no valid rows  
- Added safeguards for user misconfigurations such as having no Input Profiles set, either by having an empty array at the default settings or Input Data Asset  
- Added safeguards for Widgets having misconfigured game settings in Game Page  


## 1.2.1 

State: (Coming soon)  

Improvements:  

- Fixed a dependency issue on Examples/Default/WBP_EOD_Input_Window. It no longer uses /Test or /V1_1 widgets.  
- Abstracted EOD_Input_Window into C++, easing implementation process.  


# 1.3  

State: Coming Early Q2 2022  

New Features  

- More Graphics Options: Additional Settings (expandable by user)  
- More Audio Options: Additional Settings (expandable by user)  
- Example: Runtime Property Widget 

# Notes

## Features in consideration  

- Widget system: Hover tooltip viewer for descriptions.  
- Graphic System: Detect Monitors  
- Audio System: Detect Audio Output Devices  
> If you'd like to see any specific features in Engine Options, Let me know!  

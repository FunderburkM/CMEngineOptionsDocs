# ChangeLogs

# 1.1 

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

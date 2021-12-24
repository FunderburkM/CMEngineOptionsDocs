# Audio

Set your own mix, and use your own classes! Furthermore, you can set as many as you may need! You can also set volume/pitch/fade in time individually.   

Sound Settings in Sound Profile           |    Sample procedurally generated Sound widgets in 1.1
:-------------------------:|:----------------------------------------------------------:
![](https://user-images.githubusercontent.com/28312571/147318230-3d8ba747-8b68-4d2f-95c7-20d179fc83a3.png) |  ![](https://user-images.githubusercontent.com/28312571/147318314-324fdcc6-dce0-4496-8ffb-f33bbb727c00.png)

For widget implementations, check the Widget page in the documentation.  

# Technical

For more information on how it's used, check [Engine Options Subsystem](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/main/EngineOptionsSubsystem.md).  

- FEngineOptionSoundLayer  Acts as a “sound Class” representative. Stores name as ID, float and Sound Class Values. It manages Volume, Pitch, and Fade Distance
- FEngineOptionSoundProfile implements the multiple Sound Layers and a Sound Mix

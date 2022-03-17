# Widget Setup Audio (1.1+)

## Audio Slot

**Parent Class: CM_Engine_Options_Widget_Audio_Slot** 

Used to represent a specific entry of a given Sound Layer. You can have Sound::Volume, Sound::Pitch, and Sound::FadeDuration.  

You need to set the visual elements plus a couple of visual update functions. You also need to override the get function to update your visual slider :). 

Generally speaking, all you need to set up your Audio Slot Class is the following 

![image](https://user-images.githubusercontent.com/28312571/158849148-128580a5-05ff-4f60-a82f-78df552ef5f4.png)  

And the Update Function from the subsystem:  
![image](https://user-images.githubusercontent.com/28312571/158849299-a4d2bba6-3ee6-4659-b9a8-619061558f18.png)


## Audio Page

**Parent Class: CM_Engine_Options_Widget_Audio_Page** 

Auto generates/detects Audio Slots, and can generate as many widgets as there are settings for it. Check the Blueprint setup for info. If auto generation is turned off, it can auto detect Audio Slot classes added at design time.

We strongly recommend leaving the auto generated options enabled. If you wish to disable auto-gen, you can add your Audio Slot widgets at design time; the Construct Event in C++ will auto detect them if bAutoGenerate is false. When adding manually, set the sound class name and sound type at each slot level. Do not add them at runtime, as the system will not add them to the respective important events.

When doing this, all you need to set is the name of the Classes and which values in the layer you want to generate for. then, set your audio slot class and you're done!  

![image](https://user-images.githubusercontent.com/28312571/158849752-8a7e1ffd-c65f-41e1-b865-93ea1ed194bc.png)  



## 1.0 to 1.1 comparisons


![image](https://user-images.githubusercontent.com/28312571/147324182-ff470372-6b39-4042-9a3a-65c44a7a08d2.png)  
![image](https://user-images.githubusercontent.com/28312571/147324123-e08d21f2-92f6-4fb5-afe3-d7c0f8164fd0.png)

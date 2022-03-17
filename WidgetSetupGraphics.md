# Widget Setup Graphics

## Graphic Slot

**Parent Class: CM_Engine_Options_Widget_Graphic_Slot** 

Used for Quality Slot entries, such as Textures/Shadows/etc. Each entry is responsible for/modifies a specific quality setting.   

Only requires Graphical layout plus a couple of visual overrides + send functions  

![image](https://user-images.githubusercontent.com/28312571/158853540-9f5189f5-0a95-4f3b-93fe-b3b880a61d43.png)


## Graphic Slot Profile

**Parent Class: CM_Engine_Options_Widget_Graphic_SlotProfile** 

Used for Quality Profile switching which affects all quality types -and therefore is treated as the “global” switcher. Switches in between the different layers that Graphic Profile has [Low/Medium/Custom] etc  

Only requires Graphical layout plus a couple of visual overrides + send functions  

![image](https://user-images.githubusercontent.com/28312571/158853647-d55faa94-3330-4b8c-af51-5e8c32439686.png)  


## Graphic Page

**Parent Class: CM_Engine_Options_Widget_Graphic_Page** 

Auto generates Graphic Slot and Profile, and serves as the baseline for storing Graphic-related widgets. Check the Blueprint setup for info. Resolution/Frame Limit/Vsync/among others are added at the BP level.  

> **Note** 1.1 Graphic Page does require you to do a significant amount of heavy lifting. We suggest looking into [our Widget Examples](WidgetSetup.md#examples).  
> You need to set up the widget layout to your needs, plus set the send functions + override the update parameter functions. Bool Parameters include things like Vsync, float parameters include Frame rate, and string parameters include Resolution.  
> This is because we don't force the design classes of specific sub elements, and allow the user more flexibility on these elements. Not every project requires every setting to be included, plus different project may choose different ways of changing things like Gamma etc.  

We strongly recommend users leaving the auto generated options available. Users can swap widget classes with their own in default panel
We have left things like Resolution/Frame Limit/Window Mode/etc at the blueprint level to allow more freedom at the design level given that those can be implemented in many different ways. 

![image](https://user-images.githubusercontent.com/28312571/147323853-cbd4ba0b-de91-4d6b-a5fe-dd15ce25bbd2.png)

![image](https://user-images.githubusercontent.com/28312571/147323867-7e570d08-2a7e-4219-96a9-86d00a03d922.png)  

![image](https://user-images.githubusercontent.com/28312571/158854172-45b2a9a6-d4d7-445c-ad36-162cd6520b31.png)


## 1.0 to 1.1 comparisons

![image](https://user-images.githubusercontent.com/28312571/147323932-59c1a4f6-fd1a-4086-a3a6-dc90b4985df5.png)

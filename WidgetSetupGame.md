# Widget Setup Game (1.1+)

## Game Slot

> This is available in 1.2  

**Parent Class: CM_Engine_Options_Widget_Game_Slot**  

Represents an individual Game Setting. Unlike Game Page, which required you to have an entire widget dedicated for a given Profile and still, to some degree, required you to manually parse your updates and settings, here we have introduced The Game Slot. 

Game Slot can handle a single setting from any profile (And of any type, byte/float/string) on its own. you can even change what even it is listening to at runtime!  

![image](https://user-images.githubusercontent.com/28312571/158046073-1f8c4ff1-abef-4e51-8c3e-8a9f875edf30.png)  

To set its base values up, you only need to modify a couple of variables:  
![image](https://user-images.githubusercontent.com/28312571/158046152-afb7af24-468c-44f0-b03d-36e4b3d41bdb.png)  

and then set their Listening and Setting functions. Here're the examples on Widget for the [Default Example](#example-default)  
![image](https://user-images.githubusercontent.com/28312571/158046111-82f4449d-fbd2-4282-a838-f11a6d32f554.png)  

1.2 comes with:  

* `WBP_EOD_Slot_Boolean`, which can handle boolean-like (usually from byte, but can also accept float and string) parameters in a simple fashion.  
* `WBP_EOD_Slot_Slider`, which can handle modifying settings through a slider. Useful for float values like Field of View, Audio Volume, among others.  
* `WBP_EOD_Slot_StringSwitch`, whicn can handle swapping between multiple strings.  

You can see them getting used in `WBP_EOD_Game_Page`. You only need to drag your Game Slot Widget, set its parameters and you're done!  
![image](https://user-images.githubusercontent.com/28312571/158046225-0c005819-43ad-4b89-8d01-e5068b70154a.png)

## Game Page  

**Parent Class: CM_Engine_Options_Widget_Game_Page**  

Represents an individual Game Profile [for example in the default project, the Gameplay Profile that contains Perma Death and Mouse Multiplier settings]. The Individual Slots are added at the BP level.
  
Game Pages also have an additional filtering concept for Subsystem Updates.    

![image](https://user-images.githubusercontent.com/28312571/158852034-f2d5cc81-47b8-4c5b-8f76-45c6a0b3b869.png)  


> **NOTE** This filtering is extremely important! We have this double layer in conjunction with the double settings as when updating bulk parameters, subsystems fire with an "ALL" value for Settings Name. When we get this, the widget will check these defined arrays and make sure we get all the setting function updates run as needed.  

![image](https://user-images.githubusercontent.com/28312571/147324371-cbbeb562-2386-455b-a29c-8f4d643e2f8d.png)  

![image](https://user-images.githubusercontent.com/28312571/158851912-4181e49c-c14f-41c0-8119-27195c2be7be.png)  

![image](https://user-images.githubusercontent.com/28312571/158852360-3c0d407f-c6f7-48cc-82b6-4e05cee572bf.png)


  
## Game Page Collection

**Parent Class: CM_Engine_Options_Widget_Game_PageCollection**  

Contains and switches between different Pages. The Pages are added at the BP level.  

You need to set up the different pages that participate as well as the Switch functions. 

![image](https://user-images.githubusercontent.com/28312571/147324262-d31dbd1c-9331-426f-8a45-eb0c123d4203.png)



## 1.0 to 1.1 comparisons  
 
![image](https://user-images.githubusercontent.com/28312571/147324387-37f6f06c-1eaf-4682-a424-8dba6f49732b.png)

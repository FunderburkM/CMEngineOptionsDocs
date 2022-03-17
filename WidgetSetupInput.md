# Widget Setup Input 

> **IMPORTANT:** Some of these widgets do require the user to do a significant amount of heavy lifting. We recommend checking [our Widget Examples](WidgetSetup.md#examples). 

## Input Key Slot

**Parent Class: CM_Engine_Options_Widget_Input_Key_Slot**  

Represents a given binding entry; can be either Action or Axis Binding. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture).  


The Input Widget Slot Fill text dictates how the content should attempt to be filled   
![image](https://user-images.githubusercontent.com/28312571/147324574-cba60296-dbbd-4dd2-aa98-5fb6cc2985a1.png)

The graphic setup, overriding the SetInputX functions for setting text/textures [like in Input Key entry], as well as a couple of request sending + Visuals updating  

![image](https://user-images.githubusercontent.com/28312571/158856102-32b45462-30a3-46bd-a34c-9d674f1e3538.png)



## Input Key Entry

**Parent Class: CM_Engine_Options_Widget_Input_Key_Entry**   

Is what OptionsWindow uses to fill its dropdown. It can showcase raw Text for Key (From FKey) or search for Layout Config Entry (Either text or texture). 

Design required [a simple text and texture values + the SetInputContentX functions overridden.]  
![image](https://user-images.githubusercontent.com/28312571/147324508-729d4602-eb19-4811-adb8-3b1a1e1583fe.png)  

![image](https://user-images.githubusercontent.com/28312571/158856175-13c9fc0d-836b-4b40-800f-361176a6431d.png)


## Input Page

**Parent Class: CM_Engine_Options_Widget_Input_Page**  

Handles the different profile switching, generation, and Input Request handling. Input Profiles present get generated at the BP level. 

 Requires very little visual setup, along with a function registration. Example Layout:  
![image](https://user-images.githubusercontent.com/28312571/147324783-6d91b5ac-4aa2-4e73-92a9-4f30db269d23.png)

You also have the Input Generation Page, which determines in which order the Input Keys are added if we use bAutoAddInputSlotsToScrollBox. Not using this slot allows you to add them manually [for instance, based on category] at the OnInputSlotAdded Delegate. You need to set the Row Class for Input Slot. The only other thing that projects with multiple input profiles [changeable at the widget level] may require is input switching. The default setup has some auto generated profiles that you can check out. 
![image](https://user-images.githubusercontent.com/28312571/147324803-86002e00-880f-455c-83ae-a13aabe85eef.png)  

To change a profile and have everything handled for you, just run the subsystem function Input Set Profile. Example from the default implementation.  
![image](https://user-images.githubusercontent.com/28312571/147324862-82e8fcfa-b744-4984-9504-959ef3fec771.png)  

Finally, we register our window widget to the page   
![image](https://user-images.githubusercontent.com/28312571/147324902-177e8226-3e2a-4f60-82b6-9111637a9315.png)

## Input Options Window

**Parent Class: CM_Engine_Options_Widget_Input_Options_Window**  

Is the widget that pops up whenever a binding request is made, either by listening key or manual binding. Is summoned by Input_Slot by the user’s request.  

This one does require a bit of setting up, especially visually given the options that it has based on the Binding type & Results. The base bp widget has the following structure:  
![image](https://user-images.githubusercontent.com/28312571/147324622-36b707ff-8ab7-4acb-a8bc-5e43cb7d3b5c.png)

We also need to run a few functions for sending requests as well as handling the visuals to achieve nicer results. That said, there are only 2 setups that matter. For sending the manual requests:  
![image](https://user-images.githubusercontent.com/28312571/147324671-1c013585-5794-4517-ba6a-63a4cdf104a9.png)

The second part: widget creation  
![image](https://user-images.githubusercontent.com/28312571/147324704-27be2673-32af-49c2-aa92-3afb2d4f9b53.png)  
![image](https://user-images.githubusercontent.com/28312571/147324718-13f3e82c-5492-42ce-8094-b1d6b3119f5f.png)

This way we’ll be able to spawn our custom widget instead of just having the classical generic string widget that ComboBoxString offers.  
![image](https://user-images.githubusercontent.com/28312571/147324749-9b133ff0-54a9-44b4-ad40-c34cad476869.png)


## 1.0 to 1.1 comparisons 

![image](https://user-images.githubusercontent.com/28312571/147324922-9943fc9f-0772-4adf-ae93-0da9dc57af95.png)

# Creating your own game settings

We recommend that you have checked [Understanding Data Assets](/UnderstandingAssets.md)  at at least a little bit of [How Settings Work](/HowSettingsWork.md). That said, let's go!

First, let's create our subclass. We recommend creating a class straight from `UUOGameSettings`, but you can choose to subclass `UUOGameSettingsFromV1` if you wish to continue from the support of the v1 types in case you had v1 save game settings. **We will first cover this conceptually using the Blueprint version, then show an example for C++**.  

![Image](/Resources/Assets/SS_ClassPicker_GameSettings.JPG)  

With the type created, let's go over a few things. The `why` of this is explained [here](/HowSettingsWork.md#saving-and-serialization), for now let's just do a step by step of what's needed.  

> **Important**: when creating your own type of game settings, it is crucial that you set your gameplay tag correctly! In blueprint that will mean checking your Class' default settings and setting the variable GameSettingsTag in that panel. In C++, it means setting it inside that class' constructor. More on this in the C++ section.  

![image](/Resources/Framework/SS_TestGame_Defaults-tag.JPG)  

For showcase purporses, we'll duplicate the setup of the test game settings included in the plugin. We'll be testing with a blueprint struct S_UO_HeroSettings, and make an Array Property of this struct.  

![Image](/Resources/Framework/SS_TestGame_Defaults.JPG)  

> **Note**: This entire process has been significantly simplified in v2.1+, where you can now register data by simply marking it SaveGame.  

Now, we'll need to override two basic functions `MakeDefaultValuesToJson` and `SynchronizeData`. Make sure that for both of them, you call Super (C++)/Parent (BP)!  

In MakeDefaultValuesToJson, we add the values that we want to be considered as Defaults, so we'll be setting the field `Heroes` to be our struct array.  

![Image](/Resources/Framework/SS_TestGame_MakeDefaults.JPG)  

In Synchronize Data, we have to do two things:  

* if Data Operation is `Synchronize Pending`, then we need to write to the input json to update it with our latest Heroes struct array.  
* else, we need to read from the input json to update our Heroes variable from said Json. This will happen when discarding unsaved changes, resetting to default, or when loading from save file.  

![image](/Resources/Framework/SS_TestGame_Synchronize.JPG)  

Additionally, you can implement the `ParseData` function, which allows you to generically set information to your settings from anywhere. Its dynamics are explained [here](/WorkingWithTheSettingsSystem.md#writing-data).  

![Image](/Resources/Framework/SS_TestGame_SetData.JPG)  

## In C++

In C++, say that the Hero Settings Struct was made in C++. I'm going to put the 1 to 1 example here for consistency. Remember that C++ cannot know about the types, structs, enums, classes, nor properties that are created at the editor level.  

```cpp
//the .h
#pragma once

#include "CoreMinimal.h"
#include "UOGameSettings.h"
#include "MyGameSettings.generated.h"

UCLASS()
class MYMODULE_API UMyGameSettings : public UUOGameSettings
{
  GENERATED_BODY()

public:
  UMyGameSettings();

protected:

  //You can also override things like Initialize, LoadSystem, OnBeginPlay, etc. You can check UUOSoundSettings' class for a more complete and complex setup.  

  virtual FJsonObjectWrapper MakeDefaultValuesToJson_Implementation() const override;

  virtual bool SynchronizeData_Implementation(FJsonObjectWrapper& InJsonData, EUODataOperation DataOperation) override;

  //and for the optional ParseSetData
  virtual bool ParseSetData_Implementation(const FString& FieldName, const FUOJsonValue& InputValue) override;
};

```

```cpp
//the .cpp 
#include "MyGameSettings.h"
#include "UOJsonUtilities.h"
#include "MyNativeTags.h"

UMyGameSettings::UMyGameSettings()
{
  //for an example on native tags, you can check 
  //CM_Engine_Options/Source/UniversalOptions/Public/Defaults/UONativeTags.h
  //and CM_Engine_Options/Source/UniversalOptions/Private/Defaults/UONativeTags.cpp
  GameSettingsTag = MY_NATIVE_TAG;
}

FJsonObjectWrapper UMyGameSettings::MakeDefaultValuesToJson_Implementation() const
{
  FJsonObjectWrapper Result = Super::MakeDefaultValuesToJson_Implementation();

  TArray<TSharedPtr<FJsonValue>> Array;
  UUOJsonUtilities::MakeJsonArray(AvailableHeroes, &Array);
  Result.JsonObject->SetArrayField(gameSettings, Array);
  return Result;
}


bool UMyGameSettings::SynchronizeData_Implementation(FJsonObjectWrapper& InJsonData, EUODataOperation DataOperation)
{
  Super::SynchronizeData_Implementation(InJsonData, DataOperation);

  if (DataOperation == EUODataOperation::SynchronizePending)
  {
    TArray<TSharedPtr<FJsonValue>> Array;
    //This is a utility function that converts a TArray<a struct with our UO_TO_JSON functions to a json value array>
    UUOJsonUtilities::MakeJsonArray(AvailableHeroes, &Array);

    InJsonData.JsonObject->SetArrayField(TEXT("Heroes"), Array);
  }
  else
  {
    const TArray<TSharedPtr<FJsonValue>>* Array;
    if (InJsonData.JsonObject->TryGetArrayField(TEXT("Heroes"), Array))
    {
      //This is a utility function that converts to an TArray<a struct with our UO_FROM_JSON functions from a json value array>
      UUOJsonUtilities::MakeArrayFromJson(AvailableHeroes, Array);
    }
  }
  return true;
}

bool UMyGameSettings::ParseSetData_Implementation(const FString& FieldName, const FUOJsonValue& InputValue)
{
  if (Super::ParseSetData_Implementation(FieldName, InputValue))
  {
    return true;
  }
  //
  if (FieldName == TEXT("Heroes"))
  {
    const TArray<TSharedPtr<FJsonValue>>* Array;
    if (InJsonData.JsonObject->TryGetArrayField(TEXT("Heroes"), Array))
    {
      //This is a utility function that converts to an TArray<a struct with our UO_FROM_JSON functions from a json value array>
      UUOJsonUtilities::MakeArrayFromJson(AvailableHeroes, Array);
      return true;
    }
  }
  return false;
}
```  

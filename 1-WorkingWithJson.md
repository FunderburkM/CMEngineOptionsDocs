# Working with JSON

## JSON Paths

We utilize the concept of `JSON Paths` throughout our systems. In short, a json path gives us a way to navigate through a `JSON` Object. Let's go with the following example:  

```json
{
    "myFloat" : 42.0,
    "myObject" : {
        "myBool" : true
    },
    "myArray" : [ 
        0, 
        24, 
        "somestring",
        {
            "myString" : "hello world"
        }
    ]
}
```

* For accessing "myFloat", `$.myFloat` outputs `42.0`  
* For accessing "myBool", `$.myObject.myBool` outputs `true`  
* for accessing "myArray", `$.myArray` outputs `{ 0, 24, "somestring", { "myString" : "hello world" }}`  
* for accessing "myString", `$.myArray[].myString` outputs `hello world`  

If you wish to play around with this further, you can check this [nice tool](https://jsonpath.com/).  

For our plugin, we use this with `FUOJsonPath`. More specifics in [Working with Paths](/1-WorkingWithTheSettingsSystem.md#working-with-paths).  

## Workflow

In C++, we recommend checking our `UUOJsonUtilities` Function library. Your ustructs can just drop to have the conversion functions ready to go.  

```cpp
UO_TO_JSON();
UO_TO_JSONVALUE();
UO_FROM_JSON();
```

> Note: V2.1 doesn't need to add these functions anymore, and are now an optional method for specifying json reading/writing that does not depend on UPROPERTY.  

### Working with JSON Objects

You can check FJsonObject's documentation [here](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/FJsonObject/), and FJsonObjectWrapper's documentation [here](https://docs.unrealengine.com/4.27/en-US/API/Runtime/JsonUtilities/FJsonObjectWrapper/).  

In C++, using the utility functions, for example when working with arrays:  

> Note: V2.1 has completely changed working with JSON in C++  

```cpp
//from struct to json
TArray<TSharedPtr<FJsonValue>> Array;
UUOJsonUtilities::MakeJsonArray(LegacyGameSettings, &Array);
InJsonData.JsonObject->SetArrayField(gameSettings, Array);

//from json to struct
const TArray<TSharedPtr<FJsonValue>>* Array;
if (InJsonData.JsonObject->TryGetArrayField(gameSettings, Array))
{
    UUOJsonUtilities::MakeArrayFromJson(LegacyGameSettings, Array);
}
```

Generally speaking:  

```cpp
//to access the json object inside the wrapper
TheJsonWrapperObject.JsonObject;
//so for example
if (TSharedPtr<FJsonValue> JsonValue = TheJsonWrapperObject.JsonObject.TryGetField("theNameOfMyField"))
{
    //do something with this.
}

//for writing, you just do 
TheJsonWrapperObject.JsonObject->SetField(TEXT("theNameOfMyField"), AJsonValue);

//you can also do specific queries, like for reading
bool bThebool;
if (TheJsonWrapperObject.JsonObject->TryGetBoolField(TEXT("theNameOfMyField")))
{
    //if you're looking for a value paired to "theNameOfMyField" to be of a specific type.
}

//and for writing
TheJsonWrapperObject.JsonObject->SetBoolField(TEXT("theNameOfMyField"), true);

//More information 
//https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/FJsonObject/
```

More information on how to use GetField functions and SetField functions, check the `FJsonValue` below in the JSON value section.  

In Blueprints, you can either work with our `FUOJsonValue` (check the section below) and convert things to json value before interacting with `JSON` objects, or you can use the default functionality in the engine's `Json Blueprint Utilities` plugin  
![Json Object](/Resources/Framework/SS_Graph_JsonObject.JPG)  

### Working with JSON Values

You can check [this documentation page](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/) for information on Json Values and how they operate.  

In C++, you can check our utility functions in `UUOJsonUtilities`, but most of the functionality related to values is in our struct wrapper `FUOJsonValue`, a wrapper for `TSharedPtr<FJsonValue>`.  

You usually create shared instances of the objects, for example:  

```cpp
//Creating an Array
TArray<TSharedPtr<FJsonValue>> TheJsonValueArray = {Some Valid Value};
TSharedPtr<FJsonValue> TheJsonValue = MakeShared<FJsonValueArray>(TheJsonValueArray);

//Creating an Object
TSharedPtr<FJsonObject> TheJsonObject = {Some Valid Value}; 
TSharedPtr<FJsonValue> TheJsonValue = MakeShared<FJsonValueObject>(TheJsonObject); 

//Creating Primitive types
//Some Number can be signed/unsigned integers, floats, double
double SomeNumber = 42;
TSharedPtr<FJsonValue> TheJsonValue = MakeShared<FJsonValueNumber>(SomeNumber);
bool bSomeBoolean = true;
TSharedPtr<FJsonValue> TheJsonValue = MakeShared<FJsonValueBoolean>(bSomeBoolean);
FString SomeFString = TEXT("Hello");
TSharedPtr<FJsonValue> TheJsonValue = MakeShared<FJsonValueString>(SomeFString);
```

Then you can set that value as a field:  

```cpp
//Say that we have a json wrapper
InJsonWrapper.JsonObject->SetField(TEXT("theNameOfMyField"), TheJsonValue);
```

Or save yourself the trouble of doing `MakeShared<>` manually by using one of `FJsonObject`'s functions. For example doing it from SomeNumber would just be:  

```cpp
InJsonWrapper.JsonObject->SetNumberField(TEXT("theNameOfMyField"), SomeNumber);
```

In Blueprints, you can check the functionality mentioned above from Epic's custom nodes from `Json Blueprint Utilities`, or if you want to work with `FUOJsonValue`, you can check our `UUOJsonBPUtilities`  
![Search](/Resources/Framework/SS_Graph_JsonValue.JPG)  

To convert from and to any type, you can use the following  
![ToType](/Resources/Framework/ss_graph_JsonValue-2.JPG)  

You can also use the specific functions, like `From Json Value (to Bool)` and the like.  

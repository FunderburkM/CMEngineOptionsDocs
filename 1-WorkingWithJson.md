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

### Working with JSON Objects

You can check FJsonObject's documentation [here](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/FJsonObject/), and FJsonObjectWrapper's documentation [here](https://docs.unrealengine.com/4.27/en-US/API/Runtime/JsonUtilities/FJsonObjectWrapper/).  

In C++, using UUOJsonUtilities, all functions and macros have been reduced to two actions:  

```cpp

template<typename T>
static bool GetField(TSharedPtr<FJsonObject> or FJsonObjectWrapper, FieldName, T& OutValue)

template<typename T>
static void SetField(TSharedPtr<FJsonObject> or FJsonObjectWrapper, FieldName, const T& InValue)

UO_GET_FIELD(JsonObject, FieldName, Output)
UO_SET_FIELD(JsonObject, FieldName, Input)
```

for both Get Field and Set Field, these functions support primitives, FName/FString, Structures, FProperty*, Arrays, Maps, and even types with custom methods!

```cpp

int32 Output;
//float Output;
//FString Output;
//FName Output;
//FMyStruct Output;
//TArray<FMyStruct> Output;
//TMap<FString, FMyStruct> Output; //or TMap<FString, int32, etc>
//FUOJsonValue Output;
//TSharedPtr<FJsonObject> Output;
//TSharedPtr<FJsonValue> Output;
//FJsonSerializable Output;
if (UO_GET_FIELD(JsonObject, TEXT("myField"), Output))
{

}

UO_SET_FIELD(JsonObject, TEXT("myField"), FVector(50.f, 55.f, 65.f));

```

for structs and custom classes (non-uclasses), you can also implement your own functions! if your UStructs don't have these functions declared, it'll use the engine's default JSON-ToStruct and Struct-ToJson functionality.  

```cpp
TSharedPtr<FJsonObject> ToJson() const;
TSharedPtr<FJsonValue> ToJsonValue() const;
bool FromJson(const TSharedPtr<FJsonObject>& Input); 
```

You can still work with our settings using V2.0's and the engine's default methods.  
> [Please check V2.0's page on Json default workflow](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/V2.0-Docs/1-WorkingWithJson.md#working-with-json-objects).

In Blueprints, you can either work with our `FUOJsonValue` (check the section below) and convert things to json value before interacting with `JSON` objects, or you can use the default functionality in the engine's `Json Blueprint Utilities` plugin  
![Json Object](/Resources/Framework/SS_Graph_JsonObject.JPG)  

### Working with JSON Values

You can check [this documentation page](https://docs.unrealengine.com/4.27/en-US/API/Runtime/Json/Dom/) for information on Json Values and how they operate.  

In C++, you can check our utility functions in `UUOJsonUtilities`, but most of the functionality related to values is in our struct wrapper `FUOJsonValue`, a wrapper for `TSharedPtr<FJsonValue>`.  

> [You can check V2.0's page on working directly with JsonValues, with V2.1+ you don't need to interact directly with them most of the time](https://github.com/FunderburkM/CMEngineOptionsDocs/blob/V2.0-Docs/1-WorkingWithJson.md#working-with-json-values).  

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


In Blueprints, you can check the functionality mentioned above from Epic's custom nodes from `Json Blueprint Utilities`, or if you want to work with `FUOJsonValue`, you can check our `UUOJsonBPUtilities`  
![Search](/Resources/Framework/SS_Graph_JsonValue.JPG)  

To convert from and to any type, you can use the following  
![ToType](/Resources/Framework/ss_graph_JsonValue-2.JPG)  

You can also use the specific functions, like `From Json Value (to Bool)` and the like.  

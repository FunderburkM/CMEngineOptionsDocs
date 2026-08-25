# Reference Sheet

> **What this is:** a per-type lookup of the developer-facing entry points, each with a short "what it does". It is a reference table, **not** a tutorial — for how the pieces fit together and *why*, use the numbered guides ([Working with the Settings System](/1-WorkingWithTheSettingsSystem.md), [How Saving Works](/1-HowSavingWorks.md), [Working with Widgets](/3-WorkingWithWidgets.md)). Generated from the source and kept apart from the concept docs.

**One idiom to know:** almost every mutator takes a trailing **Event** `FGameplayTagContainer` (`UniversalOptions.Action.Event`) that decides whether the change is Applied, Saved, and/or Broadcast. Ready-made options live on `UUOCoreHelper` (C++) and the container builder (Blueprint). It is omitted from the tables below for brevity.

---

## Subsystem — `UUOUniversalOptions`

Game Instance Subsystem. Reach it with `UUOUniversalOptions::Get(WorldContext)`.

### Getters

| Function | What it does |
|---|---|
| `GetSystemSettings` / `GetPlatformSettings` / `GetGraphicSettings` / `GetInputSettings` / `GetSoundSettings` | Return the built-in setting object of that type. |
| `GetGameSettings(Tag)` | Return a game-settings object by its gameplay tag. |
| `GetSettingsByTag(Tag)` / `GetSettingsByClass(Class)` | Resolve any setting object by tag or by class. |
| `GetWidgetSettings` / `GetGlobalWidgetClass` | The active widget settings and the class used to build the options panel. |
| `GetFirstAvailableSystem` | The first setting system that is enabled and loaded — handy when a hard-coded tag might be disabled. |
| `IsFeatureEnabled(Tag)` | Whether a given system is allowed to run (per Initialization Settings + Load System). |

### Actions

| Function | What it does |
|---|---|
| `SystemAction(Systems, Action, Event)` | The master verb — set-pending / discard / reset a set of systems, then Apply / Save / Broadcast per the Event. |
| `ClearSaveFlags(Systems)` | Delete the saved data for those systems without changing the applied runtime values. |
| `HasPendingChanges(Systems)` | Whether any of those systems has unset, unsaved, or unapplied changes. |
| `RestartSystemWithAsset(Asset)` | Hot-swap the loaded settings asset at runtime. Registered fragments survive by default. |
| `RegisterSettingsFragment` / `UnregisterSettingsFragment` | Add or remove a whole fragment of game settings at runtime (Game-Feature style). Returns an `FUORegistrationHandle`. |
| `RegisterGameSetting` / `UnregisterGameSetting` | Single-setting convenience versions of the above. |

### Events (BlueprintAssignable)

| Event | Fires when |
|---|---|
| `OnSettingsObjectUpdated` | A setting object broadcasts a change. |
| `OnSystemLoaded` | The system finished loading its settings. |
| `OnSystemRestarted` | `RestartSystemWithAsset` completed. |
| `OnGameSettingRegistered` / `OnGameSettingUnregistered` | A game setting was added to or removed from the live catalog. |

### Project Settings (Global config)

| Field | Purpose |
|---|---|
| `UniversalSettings` | The settings asset loaded at startup. |
| `UniversalOptionsSaveGameName` | The save file name. |
| `InitializeSettings` | Which systems are allowed to auto-load/apply (e.g. skip graphics in editor). |
| `bForceSyncOnShutdown` | Flush every slot on exit so async saves aren't lost during teardown. |
| V1 backward-compat block | `bTryToLoadV1SaveFile`, `V1RecoveryModules`, `V1SaveFileName`, `bDeleteV1SaveFileAfterSavingToV2`. |

---

## Settings base — `UUOSettingsBase`

Common to every setting object.

### Actions and data

| Function | What it does |
|---|---|
| `SetPendingChanges(Event)` | Commit pending edits to confirmed, then act per Event. Returns whether it changed. |
| `DiscardPendingChanges` / `ResetToDefault` | Roll back to the last confirmed / to defaults. Each returns whether it changed. |
| `HasPendingChanges` | Whether there are unapplied edits. |
| `AddPendingBroadcast(Path, Event)` | Queue a JSON path for the next broadcast, or broadcast it immediately (single). |
| `GetData(Path, out Value)` / `SetData(Path, Value, Event)` | Read or write a value by JSON path (auto-converts to your type). |
| `GetAsJson` / `GetAsJsonString` | The setting's current data as a JSON object / string. |
| `GetFloatRangeValue` / `GetIntRangeValue` | Query a value's valid range (override point per setting type). |
| `RegisterListener(Object, bAdd)` | Subscribe/unsubscribe an object implementing `IUOSettingsUpdateInterface`. |

### Override hooks (for subclasses)

| Hook | Called for |
|---|---|
| `Initialize` | One-time setup (load data assets here). |
| `MakeDefaultValuesToJson` | Define the default JSON (what Reset restores). |
| `SynchronizeData` | Read/write your variables to/from JSON (pending, revert, default, load). |
| `LoadSystem` / `PreLoadSystem` | Load-time setup, before and after data is in place. |
| `ExecuteApplySettings` | Reflect the settings onto the world (apply graphics, rebind input, …). |
| `ParseSetData` | Handle a generic `SetData` write for manually-registered properties. |
| `ApplyRedirectsOnLoad` | Migrate renamed fields when loading old saves. |
| World / begin-play lifecycle | `OnBeginPlay`, `OnWorldInitialized`, `OnWorldCleanup`, `OnActorsInitialized`, `Deinitialize`. |

### Notable fields

| Field | Purpose |
|---|---|
| `bEnableProcessSaveGameProperties` | Turn on automatic saving of `SaveGame`-tagged properties. |
| `bEnableDeepAnalysisOfSaveGameProperties` | Recurse into structs so only `SaveGame` members are serialized. |
| `SaveTypeTag` | Which [save backend](/1-HowSavingWorks.md#save-backends-save-models) this setting uses. |
| `FallbackLoadSlotNames` | Slot names to try on load for transparent migration from an old save. |

---

## Helper libraries

### `UUOUniversalOptionsHelper`

| Function | What it does |
|---|---|
| `Get(WorldContext)` | The subsystem. The node **Get Universal Options (Validated)** adds a valid/invalid exec pin. |
| `RegisterSettingListener(…, SettingType, SettingName, Handle, RegSettings, bFetchOnRegister)` | Subscribe a delegate to a setting's tag + JSON path — no interface or component needed. `bFetchOnRegister` delivers the current value immediately. |
| `UnregisterSettingListener(…, SettingType, SettingName, Object)` | Remove one subscription. |
| `UnregisterSettingListener_All(…, Object)` | Remove every subscription made by `Object`. |
| `GetSettingFloatRangeValue(…, SettingType, RangeTag, Specifier, out Range)` | Look up a value's valid range (e.g. `…Graphics.Range.FieldOfView`) to build a slider; `Specifier` narrows it (e.g. a sound profile). `false` if none. |
| `GetSettingIntRangeValue(…)` | Integer version of the above. |
| `GetRuntimePlatform` / `GetRuntimeGraphics` | The platform / graphics-device class currently running. |
| `BroadcastSettingDelegate(…, SettingType, SettingName, Value, bSingle)` | Manually fire a change to listeners; `bSingle` sends it now instead of queueing. |
| `GetSaveGameName` | The configured save file name. |

### `UUOSettingsBPHelper` — generic get/set by JSON path

Wildcard nodes that read or write **any** setting value by JSON path with no typed getter. The value pin accepts any type. `InJsonPath` takes `$.path` or `path`. Each returns whether the object changed.

| Node | What it does |
|---|---|
| `Set Settings Data(Object, Path, Value, Event)` | Write `Value` at `Path` on a settings object, then act per Event. |
| `Get Settings Data(Object, Path, out Value)` | Read the value at `Path` off a settings object. |
| `Set Subsystem Data(…, SettingsTag, Path, Value, Event)` | Same as Set Settings Data, but resolves the object from `SettingsTag` first. |
| `Get Subsystem Data(…, SettingsTag, Path, out Value)` | Same as Get Settings Data, resolving the object from `SettingsTag`. |

### `UUOCoreHelper`

| Function | What it does |
|---|---|
| `DoesPlatformMatch` / `DoesPlatformMatchFlags` / `GetPlatformType` | Test or read the running platform (single value or bitmask). |
| `DoesGraphicDeviceMatch` / `GetGraphicsDeviceType` | Same for the graphics-device class. |
| `CanSwitchGoRight` / `CanSwitchGoLeft` | Interpret an `EUOSwitchResultType` to enable/disable arrow buttons. |
| `GetVideoCardName` / `GetCPUName` / `GetCPUBrand` | Hardware identity strings. |
| `ConvertFloatToText` / `FloatRangeToVector2D` | Formatting / range conversion helpers. |

> Many `GetEvent_*` (ready-made Event containers) and cycle/loading helpers here are C++-only.

### `UUOWidgetHelper`

| Function | What it does |
|---|---|
| `CreateGlobalWidgetFromUniversalOptions` | Build the whole options panel from the active settings. |
| `CreateGlobalWidgetFromGlobalSettingsAsset` | Same, from a specific global widget settings asset. |
| `CreateWidgetFromSettingsAsset` | Build the widget for one settings section. |
| `CreateSectionButtonForSettingsAsset` | Build a nav/section button bound to a button delegate. |
| `CreateSoundProfileWidgets…` / `CreateInputRowWidgets` / `CreateGraphicTypeWidgets` / `CreateGameWidgetsFromGameWidgetSettings` | Per-area row builders used inside panels. |

### JSON — `UUOJsonBPUtilities` (Blueprint)

| Function | What it does |
|---|---|
| `Make Json Value (From Any Type)` / `From Json Value (To Any Type)` | Wildcard convert any type to/from `FUOJsonValue` (typed variants also exist). |
| `Get Field` / `Set Field` | Read/write a field on an `FJsonObjectWrapper`. |
| `MakeJsonObjectFromProperties` / `FillPropertiesFromJsonObject` | Reflect a UObject's properties to/from JSON in one call. |
| `MakeJsonPath` / `JsonValue_IsValid` / `JsonValue_IsNull` | Path and value helpers. |

> The C++ side (`UO_GET_FIELD` / `UO_SET_FIELD`, `ToJson` / `FromJson` in `UUOJsonUtilities`) is covered in [Working with JSON](/1-WorkingWithJson.md).

---

## Save backends — `UUOSaveModel`

Stateless strategy for how a slot is persisted. See [Save Backends](/1-HowSavingWorks.md#save-backends-save-models).

| Class / member | What it does |
|---|---|
| `UUOSaveModel_Local` | Built-in **synchronous** local save (writes on the same frame). |
| `UUOSaveModel_LocalAsync` | Built-in **asynchronous** local save (engine async save/load). |
| `Load` / `Save` / `Delete` | Async-shaped load, store, and remove for a slot (report through a callback). |
| `SaveSync` / `SupportsSyncSave` | Blocking write for the shutdown flush; override and return `true` in a custom backend. |
| `SaveModelOverrides` (on Platform asset) | Map a `SaveType` tag → a `UUOSaveModel` class to swap backends (e.g. a custom cloud model) per platform. |

---

## Per-area settings

### Graphics — `UUOGraphicSettings`

| Group | Functions |
|---|---|
| **Capability queries** (build platform-aware UI) | `IsAutoDetectEnabled`, `IsAllowedToManageResolution`, `IsAllowedtoSetVSync`, `IsAllowedToSetGamma`, `IsAllowedToSetVariableFrameLimits`, `IsAllowedToManageActiveDisplay`, `AreGraphicQualityTypesOverriddenByPlatform` — each returns whether that control is permitted on the current platform. |
| **Getters** | `GetActiveProfile`, `GetActiveGraphicsQuality`, `GetWindowMode`, `GetCurrentResolution`, `GetGamma`, `GetVSync`, `GetDynamicResolution`, `GetFrameRate`, `GetFieldOfView`, `GetResolutionScale`, `GetMonitors` / `GetCurrentMonitor`. |
| **Mutators** (each takes an Event) | `SetWindowMode`, `SetResolution` / `SetResolutionAndWindowMode`, `SetGamma`, `SetVSync`, `SetDynamicResolution`, `SetFrameRate`, `SetFieldOfView`, `SetResolutionScale`, `SetActiveGraphicQuality` / `CycleActiveGraphicsQuality`, `SetActiveProfile` / `CycleActiveProfile`, `SetMonitor` / `CycleMonitor`, `SetAutoDetectProfile`. |

### Sound — `UUOSoundSettings`

| Function / event | What it does |
|---|---|
| `GetProfileValue` / `SetProfileValue(Profile, Setting, Value)` | Read/set volume, pitch, or fade for a sound profile. |
| `SetActiveDevice` / `SetActiveDeviceByName` | Switch the audio output device. |
| `GetAudioDevices` / `GetAudioDeviceNames` | Enumerate output devices for a dropdown. |
| `IsAllowedToSwitchAudioDevices` | Whether device switching is permitted on this platform. |
| `OnSoundDeviceChanged` *(event)* | The active output device changed. |
| `OnSoundDeviceListRefreshed` *(event)* | The available-device list was (re)built — rebuild your dropdown. |

### Input — `UUOInputSettings` (+ `UUORebindStandardInput`, `UUOEnhancedRebindInput`)

| Function / event | What it does |
|---|---|
| `SetActiveProfile` / `GetProfileName` | Switch or read the active input profile. |
| `IsAllowedToRebindInputs` / `IsAllowedToEditRebindingsInWidgets` | Whether rebinding is permitted / editable in widgets. |
| `GetVisualStandardInputData` / `GetVisualEnhancedInputData` | Binding display data for widgets. |
| `RequestRebindInput` *(standard)* | Begin capturing a key for a binding. |
| `OnInputControllerTypeChanged` *(event)* | Detected controller type changed (old → new) — swap prompts/profile. |
| `OnInputBindingsRebuild` *(event)* | Bindings were rebuilt (profile switch, rebind, external change). |
| `OnInputBindingRequest` / `OnInputBindingResult` *(events)* | A rebind was requested / its result (success, conflict, swap). |

### Game — `UUOGameSettings`

| Member | What it does |
|---|---|
| `Get(WorldContext, Tag, Class)` | Fetch a game-settings object by tag (and optionally cast to Class). |
| `GameSettingsTag` | The tag that identifies this game setting (set in the constructor / class defaults). |
| `GameSettingLoadingType` | When it loads: `Always` / `EditorOnly` / `Never`. |

### System / Platform — `UUOSystemSettings`, `UUOPlatformSettings`, `UUOPlatformSettingsAsset`

| Member | What it does |
|---|---|
| `SaveFilesToDeleteOnLoad` | Save slots to remove on load (cleanup). |
| `SaveFilesToRedirectToUniversalOptions` | Old save names to fold into the current save (rename recovery). |
| `UUOPlatformSettings::IsWindows/IsMac/IsAndroid/IsIOS/IsLinux` | Quick platform checks. |
| `SaveModelOverrides` *(on the asset)* | Per-platform save-backend mapping (`SaveType` tag → `UUOSaveModel`). |
| Per-platform override fields | Disable/allow auto-detect, VSync, resolution, monitor switching, rebinding, keyboard profiles, widget categories, and set per-platform graphic/input overrides. See [Platform Settings](/2-PlatformSettings.md). |

---

## Reacting to changes — the four ways

| # | Approach | Use when |
|---|---|---|
| 1 | Bind the subsystem's `OnSettingsObjectUpdated`, filter by tag | You want a broad hook and will filter yourself. |
| 2 | Implement `IUOSettingsUpdateInterface` + `RegisterListener` | Path-based response from your own class. |
| 3 | `UUOUniversalOptionsHelper::RegisterSettingListener` | One node, no interface or component. |
| 4 | The **Listen For Setting Changes** async node (`UUOListenForSettingChanges`) | A one-off Blueprint listener that cleans itself up. |

Drop-in component: `UUOSettingsListenerComponent` (a Modular Gameplay component). See [Responding to changes](/1-WorkingWithTheSettingsSystem.md#responding-to-changes).

---

## Legacy — `CM_Engine_Options` module

The V1 system (`ULPSS_Engine_Options`, `UCL_Engine_OptionsLibrary`). Its API is categorized `Universal Options|Deprecated` and is only relevant for migrating old save files. Prefer the V2 surface above for new work.

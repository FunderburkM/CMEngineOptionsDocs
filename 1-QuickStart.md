# Quick Start

The fastest path to a working options menu. Each step links to the guide that explains the *why* — if you would rather understand the framework before touching it, start with [Getting Started](/1-GettingStarted.md) instead.

This takes about five minutes and uses only what ships with the plugin.

---

## 1. Enable the plugin

Open the Plugins window, search for **Universal Options**, enable it, and restart the editor.

![Plugin Window](/Resources/Basics/SS_PluginWindow.JPG)

## 2. Point your project at a settings asset

Go to **Project Settings → Plugins → Universal Options** and set the settings data asset the system loads on startup, plus the save file name. The plugin ships with a ready-made default asset (`UOSettings_Defaults_v2-3`) — use it as-is to get moving.

![Loading](/Resources/Basics/SS_ProjSettings_Loading_Update.JPG)

> That single asset wires up graphics, sound, input, game, platform, and widget settings. What lives inside it is covered in [Understanding Assets](/1-UnderstandingAssets.md).

## 3. Press Play — settings already load and apply

Hit Play. The built-in graphics, audio, and input settings load from the save file (or fall back to defaults) and apply on the first world start. Nothing else is required for them to work — the framework is live everywhere through its Game Instance Subsystem.

## 4. Put the options menu on screen

Create the whole options panel from anywhere — a menu Blueprint, your HUD, a pause screen — with a single node:

**`Create Global Widget From Universal Options`** (on `UUOWidgetHelper`).

![Create Global Widget](/Resources/Widgets/SS_Graph_CreateGlobalWidget.jpg)

The global widget self-configures from the Widget Settings on your asset, so you do not wire up individual rows by hand. How that panel is composed is covered in [Working with Widgets](/3-WorkingWithWidgets.md).

## 5. Drive Apply / Save yourself (optional)

The shipped widgets already apply and save for you. When you drive settings from your own UI or gameplay code, everything flows through one idea: an **Event** tag container that says what to do — **Apply**, **Save**, and/or **Broadcast**.

```cpp
// Apply + save the pending changes for one or more systems.
if (UUOUniversalOptions* UO = UUOUniversalOptions::Get(this))
{
    UO->SystemAction(
        FGameplayTagContainer(UO_Settings_Graphics),   // which system(s)
        UO_Action_System_SetPending,                   // set pending -> confirmed
        UUOCoreHelper::GetEvent_ApplySave());          // then apply + save
}
```

In Blueprint the same call is available on the subsystem, and the `Event` container has ready-made options (Apply, Save, ApplySave, Broadcast). The full model — pending vs. confirmed data, per-setting actions, and broadcasting to listeners — is in [Working with the Settings System](/1-WorkingWithTheSettingsSystem.md).

---

## Where to go next

* [Getting Started](/1-GettingStarted.md) — the framework at a conceptual level.
* [How Settings Work](/1-HowSettingsWork.md) and [How Saving Works](/1-HowSavingWorks.md) — the data model.
* [Creating your own Game Settings](/3-CreatingYourOwnGameSettings.md) — add your own saved gameplay settings.
* [Reference Sheet](/4-Reference.md) — a flat list of the exposed helper functions, events, and settings.

# System Settings

System Settings cover the handling of the Universal Options system itself — chiefly how it deals with **save files** as your project evolves. It is configured through the `System Settings` instanced object on your [Settings Asset](/1-UnderstandingAssets.md).

This category grows over time; today it provides two save-file housekeeping services, and v2.4 adds a few related save controls (linked at the bottom).

## Save Files to Delete On Load

A list of save-game slot names that should be removed the next time the system loads.

Use it to clean up files you no longer want to ship or keep around — leftovers from a previous game version, a system you've removed, or a renamed save. It deletes any save game slot matching the name, so it is **not** restricted to files that Universal Options itself created; you can use it as a general "remove these old saves on startup" list.

## Redirect To Universal Options

Handles changes to your **Project Settings → Save Game File Name** over the life of the project.

Say your save file was `UniversalOptions.sav` early on, and across versions you renamed it to `MyGame.sav`, then `MyGameSettings.sav`. List those old names here and the system will try to link their contents to the current Universal Options save.

> If it finds a matching old save that Universal Options directly uses, it **redirects that content into the current save name and then deletes the old file**. This keeps a single authoritative save as your naming changes, so returning players don't lose their settings.

---

## Related save controls (v2.4)

These live outside the System Settings object but are part of the same "how saving is handled" story:

* **`bForceSyncOnShutdown`** (Project Settings → Universal Options) — flush every settings slot on application exit so pending async saves aren't lost during engine teardown.
* **Pluggable save backends** — `SaveModelOverrides` on the [Platform Settings](/2-PlatformSettings.md) asset maps a `UniversalOptions.SaveType.*` tag to a `UUOSaveModel` class (the built-in synchronous and asynchronous local models are the defaults). Each setting chooses its backend via its `SaveTypeTag`.
* **World Save (Experimental)** — for named, per-file world saves and actor persistence, see [World Save & Saveables](/3-WorldSaveAndSaveables.md).

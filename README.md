# instance

A feature-rich, all-in-one client-side Lua script built for **Rivals** on Roblox. It bundles combat assistance, visual customization, movement utilities, and quality-of-life modules into a single Linoria-based menu with full config save/load support and multiple UI themes.

**Disclaimer:** This project is provided for educational and research purposes only. Using third-party software violates Roblox's Terms of Service and may result in account termination. The authors take no responsibility for any account actions, bans, or damages resulting from the use of this script. Use at your own risk.

---

## Table of Contents

- [Features](#features)
- [How It Works](#how-it-works)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Feature Breakdown](#feature-breakdown)
- [Configs and Themes](#configs-and-themes)
- [Keybinds](#keybinds)
- [Folder Layout](#folder-layout)
- [Project Structure](#project-structure)
- [Notes and Limitations](#notes-and-limitations)
- [Credits](#credits)

---

## Features

- Silent Aim - screen-based targeting with hit-chance, hit-part, and FOV control
- Aimbot - smooth, curved camera aim with Toggle/Hold modes
- Ragebot - orbit attack positioning, void-spam cycling, prediction, custom weapon picker
- Anti-Aim - jitter/spinbot/random yaw and pitch, tilt/upside-down angles, underground mode
- ESP - boxes, fill, glow, health bar, names, distance, tools, skeleton, tracers, chams
- Hit Effects - 17 styles (Particles, Fortnite, Shockwave, Lightning, Blood, Fire, Ice, and more)
- Hit Sounds - 10 bundled sounds, plus kill sounds and disable-gun-sounds mode
- World - skybox, weather, atmosphere, lighting, custom aspect ratio
- Movement - Velocity walkspeed, Velocity fly, noclip, slide boost
- View Model - gun and arm chams, outlines, highlights, viewport offset, animation control
- Config System - save, load, and autoload named configs
- Themes - 7 built-in themes plus custom theme save/load
- Multi-Language - English, Korean, Spanish UI translations

---

## How It Works

`instance` is a modular loader that:

1. Boots a patched Linoria UI library with a cyan accent (`Color3.fromRGB(0, 200, 255)`), animated slider fill, global Z-index, and DisplayOrder `2147483646`.
2. Preloads dependencies - `Utility`, `EnumLibrary`, `FighterController`, `CameraController` from `ReplicatedStorage.Modules` and `PlayerScripts.Controllers`.
3. Binds render steps - separate `BindToRenderStep` handles for the ESP pass, aimbot, menu cursor, aspect ratio, viewmodel chams, keybind list, crosshair, and QOL smooth gameplay.
4. Hooks exposed modules - for example `Gun.StartShooting`, `Gun._Recoil`, `GameplayUtility.GetSpread`, `Melee.StartShooting`, `TracerEffect.Play`, `CameraController.Update`, and the Katana deflect module.
5. Fires server remotes - combat (`UseItem:FireServer`), device spoof (`SetControls`), and matchmaking (`JoinQueue:InvokeServer`, `Vote:FireServer`).
6. Writes local files - configs, themes, downloaded fonts (Roboto, Noto CJK, Fortnite), hit-sound MP3s, and kill-sound MP3s.

All modifications are client-side. Combat actions replicate through the same remotes the server already accepts.

---

## Requirements

A Roblox Lua executor supporting:

| Function | Purpose |
|----------|---------|
| `loadstring` / `load` | Load the script |
| `getgenv` | Global environment |
| `getcustomasset` / `writefile` / `readfile` | Fonts, sounds, configs |
| `isfile` / `isfolder` / `makefolder` / `listfiles` / `delfile` | File system |
| `hookfunction` / `newcclosure` | Module hooks |
| `Drawing` library | ESP, tracers, FOV, crosshair, watermark |
| `firetouchinterest` | Slingshot TP, tripmine bypass |
| `setfpscap` | Optional FPS cap |
| `setfflag` | Optional slingshot bypass |

**Target place:** Rivals (Roblox). The mod detector only fires in `GameId 6035872082`.

---

## Installation

1. Copy the full script into your executor's editor.
2. Execute while in a Rivals match (not in the lobby).
3. The menu opens automatically on first inject.
4. If it does not open, press **RightShift** or right **Ctrl**.
5. Toggle features, save a config from **UI Settings -> Configs**.

To auto-load a config on every inject, save it and then click **Set as Autoload** in the Configs groupbox.

---

## Usage

- **Menu toggle:** `RightShift` (default). Change via **UI Settings -> menu settings -> menu bind**.
- **Every tab** contains groupboxes with toggles, sliders, dropdowns, color pickers, and keybind pickers.
- **Color pickers** support right-click for copy/paste HEX or RGB.
- **Keybind pickers** support right-click to switch between `Toggle` and `Hold`.
- **The menu paints its own cursor** - the default Roblox cursor is hidden while the menu is open.
- **Unloading:** `UI Settings -> Unload Menu` (destroys everything cleanly).

---

## Feature Breakdown

### Combat

**Silent** (left tabbox)
- Silent aim toggle plus keybind
- Backshoot - teleport behind selected player, restore position on kill
- Anti-Katana - skip targets mid-deflect
- Hit chance slider (0-100%)
- Hit part dropdown (Head, Torso, Limbs, Closest, Random, and 20+ parts)

**Silent Customization** (left tabbox)
- Show FOV circle, outline colors 1 and 2, fill colors 1 and 2
- FOV radius (10-750), thickness, transparency, rotation, spin speed
- Animated fill, follow-muzzle toggle

**Aimbot** (right tabbox)
- Master toggle plus keybind (Toggle/Hold)
- Smoothness (0.1-10), aim curve (Linear, Expo, EaseIn, EaseOut, Cubic, Instant)
- Hit part dropdown, FOV radius (10-1000)
- Full FOV visual config

**Gun** (left groupbox)
- Rapid fire (zeroes per-shot cooldown)
- No spread / Max accuracy
- No recoil, rapid attack, no muzzle flash

**Ragebot** (right tabbox)
- Master toggle plus keybind
- Auto target (nearest on screen), attack position (default / under / custom)
- Custom orbit height, front, side, vertical, radius
- Weapon picker (primary / secondary / melee)
- Shoot attempts (1-3x), hit part, prediction plus multiplier
- Void spam (attack vs hide timer)
- Visualizer (tracer, rage status HUD, muzzle-anchored HUD option)

**Anti-Aim** (left groupbox)
- Yaw: jitter / spinbot / random
- Pitch: jitter / spinbot / random
- Angle: tilt 45 / tilt 90 / upside down / custom
- Min/max speed, min/max angle, random angle
- Underground mode (drop below map, camera preserved)

### Character

| Module | Description |
|--------|-------------|
| Profile | Level spoof, win-streak spoof (0.4s loop) |
| Name | Custom display name plus verified/premium badges |
| Skin Changer | Rebuild character from any UserId |
| FPS / MS / Region Spoof | Static value or randomized jitter |
| Movement | Velocity walkspeed (16-750), Velocity fly (BodyPosition and BodyGyro) |
| Self Material | Neon, ForceField, Glass, Ice, etc. with 3-color pulse |
| Animation Player | Play any rbxassetid locally plus server-side, jitter mode, 20 presets |

### Visuals

**ESP**
- Master toggle plus boxes (2-color gradient) plus fill plus glow
- Health bar (3-color lerp) plus names plus distance plus tools
- Skeleton (R6/R15 auto-detect, thickness)
- Tracers (2-color, thickness, bottom or center origin)
- Chams (Highlight, 2-color, transparency)
- High refresh / target Hz

**Utility ESP**
- Labels, icons, and distance for: Grenade, Subspace Tripmine, Flashbang, Satchel, Warpstone, Molotov, Smoke Grenade

**Lighting**
- Ambient, outdoor ambient, sun color, global shadows, shadow softness
- Color shift top and bottom, clock time, latitude, fog
- Environment diffuse/specular scale
- Color correction (contrast, saturation, brightness)
- Bloom modifier (intensity, size, threshold)

**Atmosphere**
- Density, haze, glare, offset, color, decay
- Ambient sound dropdown (8 presets) with volume

**Crosshair**
- Watermark with 3-color gradient text (spin and rotation)
- Crosshair lines (color, width, radius, length, spin, resize)
- Optional ammo readout
- Position mode: static or follow muzzle

### World

**Hit Effects** - 17 styles: Particles, Fortnite, Shockwave, Lightning, Blood, Fire, Ice, Hearts, Stars, Confetti, Ripple, Sparks, Neon, Void, Plasma, Glitch, Cosmic Shit.

**Hit Sounds** - Among Us, Bonk, Bruh, Fart, Minecraft, Neverlose, Osu, Stars, Rust HS, Vine, plus volume, pitch, and disable-gun-sounds.

**View Model**
- Gun and arm chams (3-color gradient, 14 materials, speed, transparency)
- Gun and arm outlines (3-color gradient)
- Gun and arm highlights
- Disable arms
- Viewport offset (X/Y/Z, +/-10 studs)
- No-animations per category (Idle, Reloading, Sprinting, Crouching, Shooting, Aiming)

**Bullet Tracers** - 10 styles (Line, Beam, Lightning, Heartrate, Chain, Glitch, Swirl, Neon, Plasma, Laser), color, duration, size, fade time, lerp speed.

**Skybox** - 23 presets (Neptune, Among Us, Nebula, Vaporwave, Clouds, Twilight, DaBaby, Minecraft, Chill, Redshift, Blue Stars, Blue Aurora, Realistic, StarsShader, Gloomy, Nebula Purple, Jungle, Spongebob, mountain scape, yellowy cloud, Aurora, winter mountain, stormy) plus Sun/Moon/Stars element toggles.

**Weather** - Rain, Snow, Light Rain, Stars, Hearts (shared color, rate, speed, size, opacity, spread, brightness, emission, glow, size scale).

**Aspect Ratio** - Custom X/Y stretch (1-13 by 10-32) with camera rebinding.

**Kill Sounds** - 3 bundled MP3s, volume and pitch, plays when you confirm a kill.

### Misc

**Auto Ban Queue** - cycles weapon vote every 1s (2 weapons).

**Auto Queue** - repeatedly fires `JoinQueue:InvokeServer` (1v1 to 5v5, ranked toggle).

**Miscellaneous**
- Noclip
- Handicaps toggle
- Anti Subspace Tripmine
- Mod detector (kicks on staff join, Rivals group only)
- Anti Flashbang
- Textures (smooth, dark, transparent)
- Shaders (Future tech, atmosphere, sun rays, bloom, DOF, color correction)
- Motion blur (amount and amplifier)
- Smooth gameplay

**Device Spoof** - Mobile, Console, VR, PC.

**Slide Boost** - direct velocity write while sliding.

### UI Settings

- Menu bind key picker
- Keybind list toggle (draggable panel)
- FPS cap (0 = uncapped)
- Language: English, Korean, Spanish
- Unload menu button
- Server tools: Rejoin, Random hop, Lowest players, Biggest server
- Theme picker plus custom theme save/load
- Config save, load, autoload

---

## Configs and Themes

**Configs** are saved as JSON at:

instance/rivals/settings/<name>.json

**Themes** are saved at:

instance/themes/<name>.json


**Autoload:** put a config name in `instance/rivals/settings/autoload.txt`.

**Built-in themes:** Default, Purple, Fatality, Mint, Tokyo Night, Quartz, Silver Lake.

**Movement state is flushed on config load** - walkspeed, fly, anti-aim, and underground all reset cleanly so loading a config never leaves you frozen.

---

## Keybinds

Every keybound module appears in the **keybind list** panel (enable in **UI Settings -> menu settings -> keybind list**). The panel is draggable and refreshes every 0.5s.

Supported modules in the list:
- Silent Aim, Backshoot, Aimbot, Ragebot
- Walkspeed, Fly
- Shaders
- Menu
- Animation Player

Each key picker supports:
- Toggle mode - press to flip state
- Hold mode - press to activate, release to deactivate
- Right-click to switch modes

---

## Folder Layout

After first run:

instance/
|-- rivals/
| -- settings/ | |-- autoload.txt | |-- default.json |-- ...
-- themes/-- ...

Hitsounds/
-- sounds/ |-- amongus.mp3 |-- bonk.mp3 |-- bruh.mp3 |-- fart.mp3 |-- minecraft.mp3 |-- neverlose.mp3 |-- osu.mp3 |-- Stars.mp3 |-- rust hs.mp3-- vine.mp3

KillSounds/
-- sounds/ |-- kill_1.mp3 |-- kill_2.mp3-- kill_3.mp3

Fonts and `.font` manifests are cached in the executor workspace root:
- `roboto_font.ttf` / `roboto_font.font`
- `instance_ui_en.otf` / `instance_ui_en.font`
- `instance_ui_ko.otf` / `instance_ui_ko.font`
- `instance_ui_es.otf` / `instance_ui_es.font`
- `esp_font.ttf` / `esp_font.font`
- `util_font.ttf`

---

## Project Structure

The script is organized into chained IIFEs and `do ... end` blocks:

Boot
|-- instanceShowLoadingNotification()
|-- instanceSafeRequire() plus module cache
|-- instancePreloadCoreModules()
|-- instanceHideErrorPromptsStep()
`-- instanceRunLoadingBootstrap()

UI Layer
|-- Patched Linoria (InstanceLibrary_Patched.lua)
|-- SaveManager
|-- ThemeManager
`-- Window -> Tabs (Combat, Character, Visuals, World, Misc, UI Settings)

Combat Block
|-- Silent aim, Backshoot, Anti-Katana
|-- Aimbot (BindToRenderStep)
|-- Gun hooks (StartShooting, _Recoil, GetSpread)
|-- Ragebot (orbit, voidspam, prediction, autoshoot)
`-- Anti-Aim (yaw/pitch/angle, underground)

Character Block
|-- Profile spoof
|-- Name spoof
|-- Skin changer
|-- FPS/MS/Region spoof
|-- Movement (walkspeed, fly)
|-- Self material
`-- Animation player

Visuals Block
|-- ESP (Drawings plus BillboardGui)
|-- Utility ESP (grenades, etc.)
|-- Lighting
|-- Atmosphere
|-- Crosshair
`-- Themes

World Block
|-- Hit effects (17 styles)
|-- Hit sounds (10)
|-- Kill sounds (3)
|-- View model (chams, outlines, viewport, no-anim)
|-- Bullet tracers (10 styles)
|-- Skybox (23 presets)
|-- Aspect ratio
`-- Weather (5 types)

Misc Block
|-- Auto ban queue
|-- Auto queue
|-- Noclip, handicaps, anti-tripmine
|-- Mod detector
|-- Anti-flashbang
|-- Device spoof
|-- Slide boost
|-- QOL (motion blur, shaders, smooth gameplay)
`-- Textures (smooth, dark, transparent)

Finalization
|-- Watermark (FPS)
|-- Keybind list panel
`-- Config and theme persistence hooks


---

## Notes and Limitations

- The menu opens with a custom drawn cursor - the real mouse icon is hidden while open.
- `setfpscap(0)` is called once on inject to remove the default FPS cap.
- The mod detector is enabled by default and only fires in the Rivals place (`GameId 6035872082`).
- Ragebot, Anti-Aim, Backshoot, and Void spam depend on `FighterController.LocalFighter.Entity` existing - they silently no-op in lobby and menu states.
- All hooks are wrapped in `pcall` and retried; if a module is not loaded yet the script waits for `PlayerScripts` and `ReplicatedStorage.Modules`.
- Unloading (`UI Settings -> Unload Menu`) cleanly disconnects the watermark, destroys menu cursor drawings, and removes the keybind list.
- File downloads on first run:
  - Roboto font
  - Noto CJK font (Korean)
  - Fortnite font (English/Spanish UI)
  - 10 hit-sound MP3s
  - 3 kill-sound MP3s
  - ESP font (smallest pixel 7)
  - Util font
  - Expect a few seconds of network fetches on the very first execute.
- Some spoofs use `SetAttribute` plus UI label rewriting - they are cosmetic only.
- Hit sound URLs are fetched from `myinstants.com` - if they die, replace the URLs in the `downloadHS` function.

---

## Credits

- Linoria by Inori - base UI library
- Roblox Rivals - target game
- Community contributors for hit-sound packs and ESP font assets
- Built and maintained by the `instance` project

---

## License

This project is provided as-is for educational purposes only. No warranty is provided. The user assumes all responsibility for how this software is used.


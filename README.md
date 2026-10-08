# First Person Overhaul IV

A **first-person mod** for **GTA IV: The Complete Edition (1.2.0.59)**. It works on foot, in vehicles, on trains and in cutscenes.

This is an improved version of the **GTA IV First-Person Mod by Thomaxius**:
https://github.com/Thomaxius/gta-iv-first-person-mod

All the base features (first person on foot, in vehicles and in cutscenes, head hiding, aim alignment and per-context tuning) come from that mod. This version adds full controller support and a much smoother car camera.

## Highlights
- **First person is part of the camera rotation.** Press the game's own camera button (touchpad on a DualSense, Back on Xbox, V on keyboard) to cycle near → normal → far → **first person**. You no longer need a separate key, though F7 still works.
- **Full controller support.** Look around with the right stick, turn your head in vehicles and use the look-behind button. Everything works with a DualSense or Xbox controller as well as mouse and keyboard.

## What this version adds
**Controller**
- First person becomes one more step of the game's own camera button (touchpad on a DualSense, Back on Xbox, V on keyboard): near → normal → far → **first person** → near. In vehicles it comes after the hood camera (`CarFirstPersonAfter`).
- The right stick and the mouse both look around, using the game's own sensitivity and invert settings. Walking always goes where you look (`FollowGameCameraOnFoot`).
- In vehicles, the right stick turns your head up to 150° to the right and 130° to the left. The view springs back when you let go (`CarAutoCenter`), and the look-behind button shows the rear window. Looking right also leans toward the middle of the car so you can see out.
- If you stand still and look far away from where Niko faces, he turns around smoothly (`TurnBodyWithView`).

**Driving camera**
- The eye stays on Niko's head at any speed. The head bone lags the car by a frame or two, and the mod measures and compensates that lag, so the body no longer slides into view.
- G-forces: braking and crashes push the view forward, and corners push it sideways. In a crash the view follows where Niko's head is thrown.
- The measured seated pose is saved in `[Internal]` in the `.ini`, so the first car of a session is right from the start.

**On foot and combat**
- Melee view: the locked-on opponent stays near the center of the screen, and Niko's arms stay drawn up.
- The FOV follows the game's own FOV but eases every change in, so pitching the camera no longer gives a fish-eye effect. `BaseFov` sets the level view.
- Looking down shows your body and feet instead of the inside of the neck.
- Per-weapon eye height while aiming (`[AimEyeHeight]`), in addition to the original per-weapon eye forward offset.

**Technical**
- It builds with clang / MinGW as well as MSVC. Structured exception guards are emulated with a vectored handler.

## Requirements
- GTA IV: The Complete Edition **1.2.0.59**.
- An ASI loader. [FusionFix](https://github.com/ThirteenAG/GTAIV.EFLC.FusionFix) includes one.

## Installation
Download the latest release, then copy everything inside `Main files` into your game folder, the one that contains `GTAIV.exe`. This puts `FirstPersonCutscene.asi` and `FirstPersonCutscene.ini` in `plugins`. The file keeps its original name so existing `.ini` settings carry over.

### Recommended: better driving animations
For the best driving experience in first person, also install **[Better First Person Driving Animations](https://www.nexusmods.com/gta4/mods/1232)** by **bigchunkusamogus**. With it, the steering animations finish fully and blend into the next input. It isn't included here, so download it from the author's page.

## Keys
| Key / button | Action |
|---|---|
| Camera button (touchpad / Back / V) | Cycles through the cameras; first person is one of the steps |
| F7 | First person on / off |
| F5 | Save your current settings to the `.ini` |
| Ctrl + F5 | Reload the `.ini` |
| Ctrl + F7 | Debug mode, which unlocks live tuning keys (see the original README) |

## Settings
Everything is in `plugins\FirstPersonCutscene.ini`, with a comment above each option.
- `[Controller]` holds the settings this version adds: `CameraCycle`, `FollowGameCameraOnFoot`, `CarStickSpeed`, `CarLookMaxLeftDegrees` / `CarLookMaxRightDegrees`, `CarAutoCenter`, `CarFirstPersonAfter`, `TurnBodyWithView` and `InvertY`.
- `[OnFoot]`, `[Car]` and `[Train]` set the eye height, eye forward offset and FOV for each context (plus `BaseFov` on foot).
- `[Aim]`, `[AimEyeForward]` and `[AimEyeHeight]` control aiming.

## Known issues
- During cutscenes the wrong actor is sometimes chosen. Use debug mode with 7 / 8 to pick another one.
- Large weapons can clip a little while shooting. Tune them per weapon slot in `[AimEyeForward]`.
- The head casts no shadow, because it's hidden.

## Building
The source is a single file, `src/FirstPersonCutscene.cpp`.

MSVC (x86 developer prompt):
```
cl /LD /MT /EHsc /O2 /Fe:FirstPersonCutscene.asi FirstPersonCutscene.cpp /link /MACHINE:X86 user32.lib
```
clang / MinGW:
```
clang++ --target=i686-w64-mingw32 -msse2 -mfpmath=sse -fms-extensions -fasm-blocks -fdeclspec -O2 -shared -static -static-libgcc -static-libstdc++ -s -o FirstPersonCutscene.asi FirstPersonCutscene.cpp -luser32
```

## Credits
- **Thomaxius** made the original [GTA IV First-Person Mod](https://github.com/Thomaxius/gta-iv-first-person-mod), which this version is built on.
- **C06alt** made the first-person mod that inspired the original, and **Zolika** made the trainer whose cutscene camera inspired it.
- **bigchunkusamogus** made [Better First Person Driving Animations](https://www.nexusmods.com/gta4/mods/1232), the recommended companion mod (not included).
- The **FusionFix** team.
- First Person Overhaul IV by Matias, with AI assistance (Claude).



## Overview
This project demonstrates two procedural systems built inside Unreal Engine 5:  

1. **Procedural Weather Audio + Visual System** using MetaSounds.  
2. **Loudness-Driven Cube** using Unreal’s Audio Synesthesia analysis.  



---

## 1 Procedural Weather System (MetaSounds)

###
A custom MetaSound called **MS_Weather** generates a continuous ambient noise similar to wind.  
Inside the MetaSound graph:
- A **Noise node** provides the base sound source.  
- Its tone and amplitude are filtered through a **SV Filter (Low-Pass)** to make it feel like shifting weather.  
- The filter cutoff frequency and overall gain are driven by an exposed float input named **`Intensity`**.
  
That `Intensity` value is set dynamically inside the **WeatherController Blueprint**.  
The Blueprint stores the current intensity (0–1 range) and updates it every frame based on player input.  
When the player **holds W**, intensity rises; holding **S** lowers it.  
The value is multiplied by `Delta Seconds` each Tick and clamped between 0 and 1, giving smooth real-time changes instead of instant jumps.  
The same value is sent simultaneously to:
- the MetaSound (`Set Float Parameter "Intensity"`)  
- and a **Material Parameter Collection (MPC_Weather)** controlling visuals.
  
### Visuals
A procedural **material** uses the scalar parameter `WeatherIntensity` from the MPC to modify:
- the speed of a panning noise texture (simulating wind motion),
- and a color lerp that shifts from calm blue tones to intense orange hues.

### Sky Integration
To make the system immersive, the material was applied to a **Sky Sphere** that surrounds the player.  
As intensity rises, the entire sky brightens and the scrolling texture accelerates.  
Because the MetaSound and material share the same input value, the visual and audio atmospheres change together.  
All parameters are generated from live input.


https://github.com/user-attachments/assets/8ae0a753-e3a8-4166-b713-e56ee91600c4

---

## 2 Loudness Cube (Audio Synesthesia)

The second prototype uses **Audio Synesthesia’s Loudness NRT** to analyse a music cue.  
A Blueprint reads the loudness data at playback time and scales a cube’s Z-axis according to the measured amplitude.  
When the music becomes louder, the cube stretches upward; when quieter, it contracts.  
This demonstrates procedural motion driven by real-time audio analysis rather than hand-keyed animation.


https://github.com/user-attachments/assets/6d58baca-705a-48a6-975a-0958029cfa4f


---

## Tools, Algorithms & APIs
- **Unreal Engine**  
- **MetaSounds** for runtime audio synthesis and parameter exposure.  
- **Material Parameter Collections** for cross-system variable sharing.  
- **Audio Synesthesia (Loudness NRT)** for amplitude analysis.  
- **Blueprint scripting** for per-frame updates and input handling.

---

## What’s Procedural
Both systems rely on **runtime data**:
- The weather ambience and visuals are generated and shaped algorithmically from a single intensity value controlled by the player.  
- The cube’s motion emerges from analysing live audio amplitude.



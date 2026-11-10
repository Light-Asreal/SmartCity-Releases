<p align="center"><img src="icon.png"/></p>

# Smart City

Smart City combines approachable city-building with a live traffic simulation.
Build roads and structures, place routes and signals, compare Standard and
SMART traffic control, and take over a vehicle in Free Drive mode. The current
public release is [v1.0.3](https://github.com/Light-Asreal/SmartCity-Releases/releases/tag/v1.0.3).

## Download and install

Windows users should download the [Smart City v1.0.3 downloader](https://github.com/Light-Asreal/SmartCity-Releases/releases/download/v1.0.3/SmartCity-v1.0.3-Downloader.exe), choose an installation folder, and let it verify the game archive before launching `SmartCity.exe`. Godot and VS Code are not required. The base game works without the optional AI package.

The optional Qwen2.5 local assistant downloads separately and runs through
`llama-server.exe` on this PC. It needs about 1.15 GB of additional storage and
may require the Microsoft Visual C++ x64 runtime on a fresh Windows install.

## Controls

`R` rotates a structure, `U` starts spawn-route placement, `I` starts
destination placement, `F` enters Free Drive for the selected car, `WASD` or
arrow keys drive, `Space` brakes, and `E` exits the vehicle. `F1` saves and `F2`
loads. While the Qwen chat field is focused, ordinary text input owns the
keyboard and these gameplay shortcuts do not activate.

## Simulation and limits

The project implements route following, signal phases, queue measurements,
stopping distance, time-to-collision, flow, density, ETA, and shockwave
diagnostics. Smart City is an educational game and simulation, not certified
traffic-engineering software. Real-world use would require measured data,
calibration, validation, safety review, and operational approval.

See [CREDITS.md](CREDITS.md) for verified third-party resources and notices.

## Play and share

The Windows standalone build is in `build/SmartCity-Windows/SmartCity.exe`. Share
`build/SmartCity-Windows.zip`; extract the whole archive before launching. The
package includes the offline AI and needs no Godot editor. See [Player guide](PLAYER_GUIDE.md).

To rebuild, use `tools/package_windows.ps1 -Godot <path-to-Godot-console.exe>`.
The Windows release template is configured under `build/export-tools/templates/`;
install a matching template and update `custom_template/release` if rebuilding
on another machine. The model/runtime are copied beside the executable, while
traffic knowledge is included in the PCK. Builds and download tools are excluded
from project resources.

Menu design references: [Cities: Skylines interface](https://cdn.akamai.steamstatic.com/steam/apps/255710/manuals/CitiesSkylines-UserManual_EN.pdf)
and [Xbox UI navigation guidelines](https://learn.microsoft.com/en-us/gaming/accessibility/xbox-accessibility-guidelines/112).
The polish uses clear primary actions, progressive disclosure, visible keyboard
focus, a consistent navy/teal palette, and scalable text.

## Original starter kit

This package includes a basic template for a 3D city builder in Godot 4.6. Includes features like;

- Building and removing structures
- Smooth camera controls
- Dynamic MeshLibrary creation
- Saving/loading
- Live traffic-light placement recommendations for persistently queued, two-way intersections
- Random real-time traffic incidents, phantom jams, and temporary road closures
- Sprites and 3D Models _(CC0 licensed)_

### Screenshot

<p align="center"><img src="screenshots/screenshot.png"/></p>

### Controls

| Key | Command |
| --- | --- |
| <kbd>W</kbd> <kbd>A</kbd> <kbd>S</kbd> <kbd>D</kbd> | Move camera |
| <kbd>F</kbd> | Camera to center |
| <kbd>Middle mouse button</kbd> | Hold to rotate camera |
| <kbd>Scroll wheel</kbd> | Zoom |
| <kbd>Left mouse button</kbd> | Place selected building |
| <kbd>DEL</kbd> | Remove building |
| <kbd>Right mouse button</kbd> | Rotate building |
| <kbd>Q</kbd> <kbd>E</kbd>  | Select a building and enable placement |
| <kbd>F1</kbd> | Save |
| <kbd>F2</kbd> | Load |

The traffic panel provides plain-language SMART advice and a **Car spawn** selector: Low, Medium, or High. Standard signals give each direction a fixed 10-second green, then use yellow and all-red clearance before switching. SMART signals choose green time from waiting queues, approaching cars, downstream space, and how long each direction has waited. Cars are denied entry at a red signal in either mode. SMART also throttles spawning when queues persist and directs new cars toward the least-loaded route. Its dashboard names the active green and red approaches and shows actual speed guidance near a signal, such as **8.0 → 5.0 km/h** toward a named road crossing. Phantom traffic is less likely in SMART mode (8% of incident rolls versus one third in Standard mode) and only starts in dense clusters. Vehicles safely attempt to overtake a slower leader with a seeded 9% chance per leader encounter. Queue leaders use full acceleration when their path clears, while following vehicles retain the modeled queue-discharge slowdown. Other random incidents include brief phantom-wave pushes, stopped vehicles, and roadworks; crashes are reported only after vehicle contact or the sustained-overlap rule. Three or more cars whose safety areas overlap continuously for 2 real-time seconds are recorded as one crash; all involved cars and the rubble display are removed after 3 real-time seconds. Physical crashes block the road with a solid obstruction until cleanup. Traffic advice and incident calculations are throttled to reduce frame cost. See [REAL_WORLD_TRAFFIC_CHANGES.md](./REAL_WORLD_TRAFFIC_CHANGES.md) for details.

### Local traffic assistant

The local assistant model starts loading as soon as its start-menu checkbox is enabled, stays loaded when gameplay starts, and is stopped to free memory when the checkbox is disabled. You can keep playing while it loads. Use **Ask the local traffic assistant** to open the chat, enter a question, and choose **Send question**. The bundled Qwen2.5 1.5B Instruct model (about 1.12 GB) runs on this PC through the included Windows runtime; no Ollama or internet connection is needed. Loading may take a little while and needs roughly 2 GB of available memory. The assistant gets live information about individual cars, road-crossing queues, traffic lights, road events, crashes, and suggestions. It also knows which Automatic-mode features are active, including how many cars are receiving signal approach speed guidance; it explains that this is not a city-wide speed limit. Replies are limited to 100 words, and the assistant uses plain labels such as **Road crossing 1** instead of map coordinates or traffic jargon. It cannot change the city or simulation for you. See [ai/THIRD_PARTY_NOTICES.md](./ai/THIRD_PARTY_NOTICES.md) for model/runtime sources and licenses.

### Start menu and graphics

The start menu uses an animated city backdrop and lets you turn the local AI assistant and crash particle effects on or off before loading the city. Particle settings are saved locally; disabling them suppresses GPU smoke, impact sparks, and crash smoke while leaving traffic simulation unchanged. Graphics settings are also saved locally. **Low** disables shadows, ambient occlusion, reflections, indirect lighting, glow, and anti-aliasing and lowers 3D render resolution. Medium and High allow individual toggles for shadows, soft shading around objects, reflections on roads and buildings, extra bounced light, and glow around bright lights. **RTX-like** enables enhanced visual lighting as a preset. This is a visual approximation only: Godot's current renderer configuration does not provide dedicated ray-tracing hardware. During play, choose **Restart simulation** and confirm to return to the start menu; pressing **Play** starts a fresh city.

### Instructions

#### 1. How to add more buildings?

Duplicate one of the existing resources in the 'structures' folder, adjust the properties in the inspector. Select the 'Builder' node in the scene and add your new resources to the 'Structures' array.

#### 2. How to adjust building models?

Select the resource of the building you'd like to change in the 'structures' folder, adjust the model in the inspector.

#### 3. How to save and load cities?

Pressing F1 during gameplay saves the current city to `user://map.res`; F2 loads that saved city. This keeps your save separate from the bundled sample map, which can still be loaded with F3.

The save is stored at `user://map.res` (under the application data directory):

> Windows: `%APPDATA%/Godot/app_userdata/Smart City/`

> Linux: `~/.local/share/godot/app_userdata/Smart City/`

> MacOS: `~/Library/Application Support/Godot/app_userdata/Smart City/`

#### 4. How to include city data in the project and load this?

You'll find a sample map in the 'sample map' folder, to load this during gameplay press F3. You can find the function that handles this as 'action_load_resources' found in the 'builder.gd' script.

### License

MIT License

Copyright (c) 2026 Kenney

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Assets included in this package (2D sprites, 3D models and sound effects) are [CC0 licensed](https://creativecommons.org/publicdomain/zero/1.0/)

### Release validation

Launch `SmartCity.exe -- --verify-release` from a writable build folder to run
the opt-in visual/AI smoke check. It captures menu, settings, gameplay, assistant,
and pause images beside the executable, asks the bundled model a traffic
question, and exits. Normal launches do not run this check.

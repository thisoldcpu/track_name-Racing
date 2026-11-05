# {track_name} Racing

## FEATURE & SYSTEM INVENTORY

**State of the Source  • 5 November 2026 • Working reference**

---

### OVERVIEW

{track_name} currently combines real-road course authoring, terrain-backed driving, an explorable globe, configurable vehicle mechanics, route-aware presentation, and recording tools on the Gear3D engine. Its distinctive direction includes **TripPlanner** for turning places into courses and **VictoryLAPP** for turning moving light into persistent artwork and a race-specific photograph.

TripPlanner has substantial implementation. VictoryLAPP has a detailed design and some supporting vehicle/rendering capabilities, but its painting and Results workflow are not implemented in the reviewed source.

The strongest current description is a course-building and driving platform with growing racing and creative systems. This review did not establish a complete competitive racing product with opponents, lap classification and Results flow.

### HOW TO READ THIS INVENTORY

* **[PRESENT]** Concrete implementation found in current source. This does not mean every feature is exposed in normal menus or verified in the installed app.
* **[PARTIAL]** Useful implementation exists, but a significant integration, coverage, presentation or product-completion step remains.
* **[PLANNED]** Explicit design or stated direction; the feature itself is not established as implemented. Existing prerequisites are identified where applicable.

This is a source-level capability inventory, not a fresh build or gameplay test. Recent source is sometimes ahead of documentation and the installed executable. For example, current source wires track arrivals despite older world notes saying otherwise. Conversely, the vehicle proof explicitly still needs an IDE rebuild and native-app verification. Older failed course acceptance is not silently promoted to a pass. Existing test reports are supporting historical evidence.

---

### NAMING AND HIERARCHY

* **Product / player experience:** {track_name}
* **Shared technology:** Gear3D
* **Named signature systems:** TripPlanner; VictoryLAPP
* **Content identities retained:** PCH - Big Sur; USVI - St. John

The numbered names below, other than established names, are descriptive inventory categories. They are not proposed product renames or claims of existing brands. TripPlanner and VictoryLAPP occupy the same system level; implementation maturity and dependency importance do not determine their naming prominence.

1. **TripPlanner** — Create and prepare courses
2. **VictoryLAPP** — Paint with moving light; photograph results
3. **World & Destination Director** — Discover, enter and leave destinations
4. **Course Runtime & Persistence** — Load, prepare and retain course state
5. **Geography, Terrain & Roadscape** — Turn source geography into usable world data
6. **Vehicle, Drivetrain & Garage** — Drive; configure and inspect mechanics
7. **Cameras, Navigation & Instruments** — See the drive and understand the route
8. **Narration & Captions** — Respond to places and run events
9. **Atmosphere & Wildlife** — Animate the surrounding world
10. **Audio & Soundtrack** — Play, mix and visualize sound
11. **Recording & Presentation** — Capture scenes and evaluate encoders
12. **Interface, Input & Settings** — Operate and personalize the experience
13. **Content & Compatibility** — Reuse assets and legacy content
14. **Gear3D Runtime & Diagnostics** — Render, measure and support everything above

Content sits alongside these systems, rather than becoming a separate copy of each:

* **PCH - Big Sur:** configured road, terrain, POIs and associated presentation.
* **USVI - St. John:** configured North Shore Road, terrain and source geography; scenery/narration completeness and installed-game acceptance remain separate.
* **User-created courses:** source-bound drafts and built candidates through TripPlanner.

---

### FEATURE CATALOG

#### 01. TripPlanner

*Ownership: authoring intent, location discovery, route editing and build workflow.*

* **[PRESENT]** Native BUILD A COURSE entry and named TripPlanner interface.
* **[PRESENT]** Pick a place on the globe, use its center, or enter coordinates.
* **[PRESENT]** Separate named start/finish searches through Photon, with locality labels, explicit match confirmation, bounded requests, caching and cancellation.
* **[PRESENT]** Discover nearby OpenStreetMap roads at 5, 15 or 30 km search radii.
* **[PRESENT]** Filter loaded roads by name; inspect a pan/zoom map and fit roads to view.
* **[PRESENT]** Select road endpoints by sliders or map clicks; add and clear via points.
* **[PRESENT]** Construct connected routes using source road topology and retain source identities, geometry evidence and route recipes.
* **[PRESENT]** Prepare a local journey between confirmed places; reject unsupported region sizes and endpoints more than 500 m from a loaded road.
* **[PRESENT]** Name, save, reopen and revise course drafts; invalidate outdated builds when the authored route changes.
* **[PRESENT]** Request supported elevation coverage; show staged build progress and download status; cancel work and retain the draft.
* **[PRESENT]** Validate a completed course candidate and offer Use built course; installation/catalog updates are handled by the shared track installer.
* **[PARTIAL]** End-to-end course generation depends on a configured external worker, native baking helper, data coverage and network services. It is not a proven self-contained release installer or a worldwide terrain service.
* **[PLANNED]** Long-distance, multi-region itineraries and itinerary-aware narration.

*Evidence: source/g3d.course.{create,create.ui,places,roads,recipe,drafts}.pas; g3d.track.{build,build.net,build.ui,install}.pas; g3d.menu.pas.*

---

#### 02. VictoryLAPP — Victory Light Art Performance Photography

*Ownership: retained moving-light artwork. Cameras remain ordinary cameras; the race director owns composition and Results owns the eventual photograph.*

* **[PRESENT]** Supporting vehicle data: stable light assembly/segment IDs, resolved world positions/directions, colors and separate tail/brake contributions.
* **[PRESENT]** Supporting renderer: emissive output and bloom; existing cameras and screenshot facilities offer integration points.
* **[PLANNED]** Record actual emitter positions and light output at simulation time.
* **[PLANNED]** Form separate illuminated strokes, preserving gaps when brake lamps turn off.
* **[PLANNED]** Retain bounded world-space history and swept geometry independently of cameras.
* **[PLANNED]** Freeze emission, walk/fly around the painting, hide/show it and clear it.
* **[PLANNED]** Compose with the ordinary camera/director, capture without gameplay HUD, and use the resulting photograph behind Results.
* **[PLANNED]** Generic moving-emitter input, allowing later artistic use beyond vehicles.
* **[PLANNED]** Explicit sample/geometry budgets, discontinuity handling, clean restart and capture-failure behavior.

*Evidence: supplied VictoryLAPP_Phase_1_Desktop_Handoff.txt; source/g3d.vehicle.visual.pas; g3d.render.vehicle.opengl.pas; g3d.render.post.opengl.pas; tests/vehicle-visual/README.md.*

---

#### 03. World & Destination Director

* **[PRESENT]** Persistent world and procedural glass globe with geographic land/cloud data.
* **[PRESENT]** Orbit, zoom, focus and return interactions; geographic destination markers.
* **[PRESENT]** Separate destination identity, historic origin and content location.
* **[PRESENT]** Destination-specific arrival targets, flight phases and preparation gates.
* **[PRESENT]** Track launch, start-gate holding/release and return-to-globe integration.
* **[PRESENT]** Native home artwork over the live world, with DRIVE, BUILD A COURSE, GARAGE, SETTINGS and QUIT entry points.
* **[PARTIAL]** Seamless flight-to-drive presentation.

*Evidence: source/g3d.{world,director,arrival,globe.navigation,globe.markers,globe.land,globe.presentation,module.builtin,menu.home,track.launch}.pas.*

---

#### 04. Course Runtime & Persistence

* **[PRESENT]** Shared, data-driven course definitions and catalog; selected-course operation.
* **[PRESENT]** Per-course projected coordinates, local origin, content paths and arrival start.
* **[PRESENT]** Background preparation and cancellation before transferring a ready course.
* **[PRESENT]** Road/terrain bake loading and validation against geometry and terrain recipe.
* **[PRESENT]** Per-course exploration progress and atomic saved-state replacement.
* **[PRESENT]** Preserve actually driven road coverage; seeking does not reveal skipped road.
* **[PRESENT]** Standing start, pause/resume, restart, distance seek and endpoint handling.
* **[PRESENT]** Constant-speed cruise and a calculated three-minute full-route demo.
* **[PRESENT]** Real and miniature/hamster scale modes and route-time calculations.
* **[PARTIAL]** Race structure currently centers on a route run and timing. A complete opponent/lap/position/championship/Results system was not established.

*Evidence: source/g3d.track.{definition,session,prepare,validate,storage,storage.vfs,bake,bakefile,rail}.pas; g3d.pch.pas; g3d.pch.commands.pas.*

---

#### 05. Geography, Terrain & Roadscape

* **[PRESENT]** OSM road ingestion, explicit source route selection and reproducible preparation.
* **[PRESENT]** Road metadata, speed-limit spans, bridge spans and checkpoint data.
* **[PRESENT]** DEM/TIFF reading, terrain mosaics, road-height baking and corridor sampling.
* **[PRESENT]** Terrain stitching and bridge-aware road geometry; reusable cached bakes.
* **[PRESENT]** Textured road materials, normal strength, exposure, scale and randomized texture variation to reduce visible repetition.
* **[PRESENT]** Procedural roadside speed signs for supported known integer mph limits; these are generated placements, not surveyed sign positions.
* **[PRESENT]** Broader geography acquisition: roads/paths, water/natural features, selected areas, places, POIs, amenities, structures, access/signs and restrictions.
* **[PRESENT]** Preserve OSM identities, tags, topology, missing dependencies, source hashes and attribution; produce bounded spatial indexes and queryable packages.
* **[PRESENT]** Runtime package validation, identity lookup and bounding-box queries; optional geography loading is connected to track preparation.
* **[PARTIAL]** Rich geography data does not automatically create visible buildings, vegetation, water systems, collision or gameplay for every source object.
* **[PARTIAL]** Terrain accuracy and coverage depend on source data. Sampled road elevations and mapped limits are not surveyed road grades or verified roadside postings.

*Evidence: source/g3d.{geography,dem.tiff,terrain.mosaic,terrain.stitch}.pas; g3d.track.{mesh,material,speed,speed.signs}.pas; source/data/ingest/jawwkk/{refresh_geography,build_geography,query_geography,build_road_track,refresh_road_data,build_poi}.py.*

---

#### 06. Vehicle, Drivetrain & Garage

* **[PRESENT]** Longitudinal vehicle model: throttle, brake, road grade, drag and acceleration.
* **[PRESENT]** Five forward gears, automatic clutch, automatic/manual transmission, shift interruption, limiter and unsafe-downshift protection.
* **[PRESENT]** Garage transmission preference; manual gear selection via console.
* **[PRESENT]** Shared ECU snapshot for motion, RPM, gear, clutch/slip and mechanical display.
* **[PRESENT]** Configurable 1–6 inline cylinders, two/four-stroke kinematics, bore, stroke, rod length, compression geometry, firing phases and ignition counts.
* **[PRESENT]** Animated engine cutaway with wireframe/ghost/bare styles, labels, block opacity, illustrative combustion and slow inspection mode.
* **[PRESENT]** Embedded procedural wireframe vehicle requiring no external vehicle asset.
* **[PRESENT]** Rigid visual placement against actual road triangles, individual wheel support/gap reporting and shared pose across main and mirror views.
* **[PRESENT]** Configurable segmented lamps, attached to the body, with brake brightening.
* **[PARTIAL]** Vehicle rendering has component/GL proof; native-app rebuild/acceptance remains.
* **[PARTIAL]** Driving follows a rail. Free steering, reverse, suspension and tire dynamics are not established. Wheel support is visual placement, not suspension.
* **[PLANNED]** Real timing/calibration ingestion and richer engine response. Current kinematics do not establish cylinder-pressure, thermal or failure simulation.

*Evidence: source/g3d.{vehicle,motor,vehicle.input,vehicle.visual,vehicle.surface,engine.display,render.vehicle.opengl,render.road.actors}.pas; g3d.menu.home.pas.*

---

#### 07. Cameras, Navigation & Instruments

* **[PRESENT]** Shared camera poses, lenses, camera stack and waypoint/spline paths.
* **[PRESENT]** Driver, chase, hamster and fly rail views; temporary glances and freelook.
* **[PRESENT]** Live rear-view rendering using the same world state.
* **[PRESENT]** Local minimap and full-route view, player position, rotation and exploration fog.
* **[PRESENT]** Combined tachometer/speed display, gear/drivetrain information and run telemetry.
* **[PRESENT]** Current speed-limit and bridge-name HUD signs.
* **[PRESENT]** Imperial/metric presentation and saved visibility, scale and position settings across the existing HUD controls.
* **[PLANNED]** The supplied first-pass HUD refinement brief: clearer hierarchy, readability and arrangement while preserving customization. This is not assumed complete.

*Evidence: source/g3d.{camera,camera.path,camera.stack,track.look,rearview,pch.minimap,racing.hud,hud.layout,speed.sign.hud,bridge.sign.hud,units,settings.catalog}.pas; source/track_name_UI_first_pass.txt.*

---

#### 08. Narration & Captions

* **[PRESENT]** Route-aware POI commentary: approach, crossing, near and on-bridge events.
* **[PRESENT]** Run-start, distance/progress milestones, quiet-stretch and finish commentary.
* **[PRESENT]** Plain/wry script variations, token substitution and recent-line avoidance.
* **[PRESENT]** Reading-time spacing, priority and talk-budget rules; factual/side claims tied to available route evidence.
* **[PRESENT]** Per-POI overrides and separate authored script banks so data refreshes do not overwrite commentary.
* **[PRESENT]** Dialogue and sound-caption channels with controls and speaker labels.
* **[PARTIAL]** Current route narration is caption output. Spoken narrator audio and a complete countdown/start coordinator remain follow-on integration.

*Evidence: source/g3d.pch.{narrator,announcer}.pas; g3d.captions.pas; g3d.engine.pas/PchNarration; data/announcer/announcer_lines.txt.*

---

#### 09. Atmosphere & Wildlife

* **[PRESENT]** Globe/arrival atmospheric presentation, cloud coverage and procedural drift, shear and detail motion tied to the paused scene clock.
* **[PRESENT]** Deterministic boid flocking, instanced gulls and shader-driven wing motion.
* **[PRESENT]** Landmark-based flock placement with terrain-clearance checks, shared main/mirror rendering and flock inspection views.
* **[PARTIAL]** Birds have documented native acceptance at inspection views; ordinary road visibility remains subtle. This is not a general wildlife ecosystem.
* **[PLANNED]** Locale/time-based weather ingestion, report discovery, normalization, source selection, cached/offline scenarios and explicit overrides.
* **[PLANNED]** Weather diagnostics first, then particle consumers and integration with atmosphere, surfaces and vehicles. Wetness, snow/ice and weather-driven traction are not established by the existing cloud renderer.

*Evidence: source/g3d.{globe.clouds,weather.motion,boids,pch.birds,wildlife,render.birds.opengl}.pas; docs/boids.md; docs/track_name_weather_plan.md.*

---

#### 10. Audio & Soundtrack

* **[PRESENT]** Windows WASAPI output, WAV decoding, multiple voices, positional stereo panning/distance attenuation and sample-rate conversion.
* **[PRESENT]** Master/music/effects/menu mixer categories with saved volume controls.
* **[PRESENT]** M3U playlist handling, grouped soundtrack selection and current track metadata.
* **[PRESENT]** Module/scene soundtrack routing, menu audio and transition sound support.
* **[PRESENT]** Live mix analysis and FFT spectrum display, separate from decorative meters.
* **[PRESENT]** Physical Quake CD recognition/playback path on Windows, with a narrow recognized-disc contract and documented shutdown constraints.
* **[PARTIAL]** MP3 decoding, real surround, output switching and separate voice controls are not implemented product features; Linux/macOS audio is placeholder code.
* **[PLANNED]** Engine sound driven by the existing ignition/mechanical telemetry.

*Evidence: source/g3d.audio.{windows,wincore,playlist,soundtrack,analysis,visualizer,transition}.pas; files.{wav,m3u,mp3}.pas; g3d.cd.win.pas.*

---

#### 11. Recording & Presentation

* **[PRESENT]** Screenshots and scene capture integration points.
* **[PRESENT]** In-engine OBS panel: recording state/time, record/stop, scene/profile selection, output size/frame rate, test action and recording-folder access.
* **[PRESENT]** OBS WebSocket connection/authentication and owned recording lifecycle; temporary configuration restoration and visible failure reporting.
* **[PRESENT]** Automated selected-course capture and prepared encoder-profile sweeps.
* **[PRESENT]** Repeatable encoder workload, unrecorded baseline and timing/recording reports.
* **[PRESENT]** Procedural title hallway, curved sections, milestone captions and feature hints.
* **[PARTIAL]** OBS features require local OBS and prepared configuration. Capture plumbing does not establish VictoryLAPP composition or a completed Results screen.

*Evidence: source/g3d.{screenshot,obs.panel,obs.capture,obs.windows,obs.socket,obs.protocol,encoder.suite,encoder.scene,titlescreen,titlescreen.generator,hints}.pas.*

---

#### 12. Interface, Input & Settings

* **[PRESENT]** Native runtime widgets, DFM-backed menu conversion, layout, themes, scrolling, focus, pointer/keyboard/controller navigation and live bindings.
* **[PRESENT]** Context-sensitive input routing for menus, console, globe and driving; pause/focus-loss handling and fresh-pedal protection on resume.
* **[PRESENT]** Rebindable controls, controller presets, XInput and Steam Input integration, device prompts/glyphs, controller capabilities and battery reporting.
* **[PRESENT]** Saved settings, Apply/Back behavior, reset/default handling and console control.
* **[PRESENT]** Windowed/borderless/exclusive display modes, monitor/resolution/refresh selection, confirmation/revert protection, VSync and frame limiting.
* **[PARTIAL]** A visible settings row or menu resource is not a working feature. Several graphics/audio options are explicitly unavailable; antialiasing selection is not implemented, and some renderer/window changes wait until next launch.

*Evidence: source/g3d.gui.*.pas; g3d.menu*.pas; g3d.input.*.pas; g3d.{steaminput,glyphs,settings,settings.catalog,video,display.windows}.pas.*

---

#### 13. Content & Compatibility

* **[PRESENT]** Virtual filesystem, loose/PAK/PK3 content paths, title discovery, content browser and asset inspection commands.
* **[PRESENT]** Shared static model contract, validated GLB import, model caching, instances, node/attachment transforms and material/image metadata.
* **[PARTIAL]** Imported-model rendering paths exist, notably gulls and development probes; this is not blanket support for every GLB material or animation.
* **[PRESENT]** Registered Quake BSP29, Quake II IBSP38 and Quake III IBSP46 map loaders.
* **[PRESENT]** Q3 shader/material processing and recovered material semantics for multiple game families, retaining original source evidence and flags.
* **[PRESENT]** Material impact-response foundation with separate instance state.
* **[PARTIAL]** Legacy MD3/MD5/3DS code and additional BSP readers offer recovery starting points; they are not all connected to the current shared runtime.
* **[PARTIAL]** Map inspection/compiler tooling includes substantial Quake lighting/VIS work; other compiler providers contain stubs. No universal map-tool claim.
* **[PLANNED]** Broader animation/importer integration and selectable bot providers. The current botlib/AAS foundation does not supply working AI opponents.

*Evidence: source/g3d.{vfs,titles,content.browser,model.manager,model.glb,model.importers,bsp,materials,materials.profiles,shader,compiler,bot,aas.link}.pas; source/idtech2.compiler.quake.pas and legacy format units.*

---

#### 14. Gear3D Runtime & Diagnostics

* **[PRESENT]** OpenGL 4.6 scene/UI rendering, camera-relative road rendering and depth partitioning for large views; shared preparation of road actors.
* **[PRESENT]** HDR scene/emission targets, bloom, highlight compression and post-processing.
* **[PRESENT]** Fixed-step simulation facilities, frame limiting, worker/thread services, resource ownership and cancellable background work.
* **[PRESENT]** Console commands, configuration variables, logging and status reports.
* **[PRESENT]** FPS/frame-time history, sampled P99/peak, CPU/core meters, download-rate history and available network statistics.
* **[PRESENT]** Hardware/adapter discovery and vendor-dependent GPU telemetry/probes; availability depends on hardware and installed drivers.
* **[PRESENT]** RenderDoc capture lifecycle/checkpoints and graphics diagnostic integration.
* **[PRESENT]** UDP ping/pong and Steam networking traffic/loss test infrastructure.
* **[PARTIAL]** Networking probes do not establish multiplayer racing, matchmaking or synchronized vehicle simulation. API bindings do not prove accelerated use.
* **[PARTIAL]** Windows/OpenGL is the implemented primary path. Direct3D 12 currently clears/presents without scene rendering; Vulkan/GLES/Metal/software are unimplemented backends. Portable units do not establish complete OS ports.

*Evidence: source/g3d.{engine,render.caps,renderapi.opengl,renderapi.direct3d,render.post.opengl,timing,framelimiter,thread,overlay,gpu,compute.probe,gputrace,net.udp,net.steam}.pas and vendor provider units.*

---

### WHAT THE EXISTING WORK MAKES POSSIBLE NEXT

**Closest integration/acceptance work:**

* Finish native-app acceptance of TripPlanner -> build -> use -> drive.
* Package the builder/runtime dependencies and make service/coverage limits clear.
* Verify the visible vehicle and lamps in the ordinary application.
* Complete the HUD refinement using the existing instruments and settings.
* Make manual shifting accessible without opening the diagnostic console.
* Add richer course narration/content using the existing route and geography data.

**New subsystems with useful foundations already available:**

* **VictoryLAPP:** authoritative lamp sampling -> history -> strokes -> freeze/inspect -> clean photograph -> Results ownership. Existing lamps, cameras and bloom help, but the central feature still needs implementation.
* **Engine audio:** connect ignition/mechanical telemetry to a sound-generation path.
* **Weather:** implement ingestion/state first; then wire existing visual consumers.
* **Geographic scenery:** translate retained source objects into actual world features.

**Larger work that should not be described as “just a little more”:**

* Free-driving physics, steering/collision/suspension and richer vehicle behavior.
* Opponent AI, competitive rules, laps/classification and complete race Results.
* Multiplayer gameplay and long-distance multi-region course preparation.
* Full non-Windows ports or alternative rendering backends.

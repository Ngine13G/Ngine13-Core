\# NGINE-13 CORE



\_A dual-plane simulation engine:\_

\- \*\*NGINE-13G\*\* — physical / gameplay engine

\- \*\*NGINE-13M\*\* — metaphysical / soul engine



This repo contains the \*\*core loop\*\*, \*\*ECS architecture\*\*, and the \*\*first live wiring\*\* between body (G) and soul (M).



---



\## Features



\### NGINE-13G — Gameplay / Physical Layer



\- Rust-based \*\*game loop\*\* with fixed-ish timestep (~60 FPS)

\- \*\*ECS World\*\* with:

&nbsp; - `Entity` handles

&nbsp; - `Position`, `Velocity` components

\- \*\*Modules\*\* (plugin-style):

&nbsp; - `HeartbeatModule`

&nbsp; - `StatsModule`

&nbsp; - `EcsDemoModule` (playable demo)

\- \*\*Systems\*\*

&nbsp; - `DebugCountSystem` — counts entities with `Position`

&nbsp; - `ConsciousnessDebugSystem` — tier counts + avg awakening

&nbsp; - `SoulAwakeningSystem` — radiating awakening pulses

&nbsp; - `SoulIntentSystem` — soul → movement intent

&nbsp; - `SoulIntentApplySystem` — applies intent into `Velocity`

&nbsp; - `SoulStatsSystem` — soul identity counts + avg awakening/corruption

\- ASCII demo:

&nbsp; - Tilemap (`#` = walls, `.` = floor)

&nbsp; - Player `@` with acceleration, friction, collision

&nbsp; - Enemies with multiple archetypes:

&nbsp;   - `Chaser`, `Coward`, `Wanderer`, `Orbiter`

&nbsp; - Orbs:

&nbsp;   - Spawn, orbit player, despawn

&nbsp;   - Increase score and trigger `GameEvent`s

&nbsp; - HUD:

&nbsp;   - Player position / camera / map size

&nbsp;   - Game status: time, score, high score, difficulty, orb count

&nbsp;   - Soul HUD: tier, awakening, contagion, resistance, corruption, memories



\### NGINE-13M — Metaphysical / Soul Layer



\*\*Core components:\*\*



\- `ConsciousnessComponent`

&nbsp; - `tier: ConsciousnessTier`  

&nbsp;   - `Loop` → pure NPC

&nbsp;   - `Seed` → latent awareness

&nbsp;   - `SoulLite` → partial inner life

&nbsp;   - `SoulNode` → full node / key soul

&nbsp; - `awakening: f32` (0.0–1.0)

&nbsp; - `contagion: f32` (awakening others)

&nbsp; - `resistance: f32` (resisting awakening)



\- `SoulComponent`

&nbsp; - `identity: SoulIdentity`

&nbsp;   - `PlayerCore`, `Predator`, `Survivor`, `Drifter`, `Neutral`

&nbsp; - `emotion: SoulEmotion`

&nbsp;   - `Calm`, `Aggression`, `Fear`, `Curiosity`, `Joy`

&nbsp; - `trajectory: SoulTrajectory`

&nbsp;   - `Dormant`, `Awakening`, `Ascending`, `Corrupting`

&nbsp; - `corruption: f32`

&nbsp; - `awakening: f32` (soul’s own sense of awakening)

&nbsp; - `memories: Vec<SoulMemory>`



\- `SoulMemory`

&nbsp; - `label: String`

&nbsp; - `intensity: f32` (0.0–1.0)



\*\*Metaphysical behavior:\*\*



\- `SoulAwakeningSystem`

&nbsp; - SoulNode entities periodically emit \*\*awakening waves\*\*

&nbsp; - Nearby non-SoulNode entities:

&nbsp;   - gain `awakening`

&nbsp;   - lose `resistance`

&nbsp;   - upgrade through `Seed → SoulLite → SoulNode`



\- `compute\_soul\_intent`

&nbsp; - Maps `(ConsciousnessComponent, SoulComponent)` into:

&nbsp;   - desired velocity `(vx, vy)`

&nbsp;   - `weight` (0.0–1.0) for blending into current motion

&nbsp; - Factors in:

&nbsp;   - tier (Loop / Seed / SoulLite / SoulNode)

&nbsp;   - identity (Predator, Survivor, etc.)

&nbsp;   - emotion (Aggression, Fear, etc.)

&nbsp;   - trajectory (Awakening, Ascending, etc.)

&nbsp;   - corruption



\- `SoulIntentSystem`

&nbsp; - Reads soul + consciousness state

&nbsp; - Emits `Event::IntentOverride { entity, vx, vy, weight }`



\- `SoulIntentApplySystem`

&nbsp; - Reads those events

&nbsp; - Blends intent into `Velocity` via ECS

&nbsp; - This is the \*\*body ← soul\*\* bridge



\- `SoulStatsSystem`

&nbsp; - Global telemetry:

&nbsp;   - counts of each `SoulIdentity`

&nbsp;   - average `awakening`

&nbsp;   - average `corruption`



\### Event Bus \& Game Events



\- `EventBus`:

&nbsp; - `Event::EngineStarted`

&nbsp; - `Event::EngineStopping`

&nbsp; - `Event::Heartbeat(u64)`

&nbsp; - `Event::QuitRequested`

&nbsp; - `Event::IntentOverride { .. }`

&nbsp; - `Event::Game(GameEvent)`



\- `GameEvent`:

&nbsp; - `PlayerCaught { time\_alive, difficulty }`

&nbsp; - `Victory { time\_alive, score, difficulty }`

&nbsp; - `OrbCollected { score, target\_score, difficulty }`

&nbsp; - `DifficultyIncreased { new\_level, new\_target\_score }`



\- `Engine::handle\_game\_event`:

&nbsp; - Reacts to `GameEvent`s at the soul-field level:

&nbsp;   - `OrbCollected`:

&nbsp;     - small awakening bump, corruption washout

&nbsp;   - `Victory`:

&nbsp;     - strong awakening surge, corruption reduced

&nbsp;   - `PlayerCaught`:

&nbsp;     - field shock: corruption +, awakening –

&nbsp;   - `DifficultyIncreased`:

&nbsp;     - slight awakening boost

&nbsp;     

This is where \*\*gameplay loops\*\* (score, difficulty, death) feed back into the \*\*metaphysical state\*\*.



---



\## Directory Structure



```text

src/

&nbsp; ecs.rs        # World, Entity, Position, Velocity, ECS helpers

&nbsp; engine.rs     # Engine, systems, event loop, metaphysical reactions

&nbsp; event.rs      # Event, GameEvent, EventBus

&nbsp; input.rs      # InputState, keyboard polling (crossterm)

&nbsp; soul.rs       # ConsciousnessComponent, SoulComponent, soul math

&nbsp; main.rs       # Entry point: builds Engine, runs it

&nbsp; modules/

&nbsp;   mod.rs      # module registration

&nbsp;   ecs\_demo.rs # NGINE-13G demo: player, enemies, orbs, HUD, rendering




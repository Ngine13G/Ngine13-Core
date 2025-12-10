NGINE-13G — Next-Gen Rust Simulation & Gameplay Engine

NGINE-13G is a Rust-based experimental simulation engine built around a modular ECS (Entity-Component-System) architecture.

Designed for:

✦ fast prototyping

✦ sandbox world simulations

✦ emergent gameplay experiments

✦ plugin-style modules

✦ clean, minimal runtime

NGINE-13G is lightweight, extendable, and appropriate for developers exploring custom engine design, simulation mechanics, or Rust-based game loops.

🚀 Features
Core Engine

Fixed-timestep game loop (~60 FPS target)

Deterministic update cycle

Modular engine structure (EngineModule trait)

Pluggable runtime modules

EventBus system for inter-module communication

ECS Architecture

Entity handles

Basic built-in components:

Position

Velocity

Expandable component system

Simple world iteration + system updates

Included Modules

NGINE-13G ships with several example modules:

HeartbeatModule — basic tick timing

StatsModule — frame timing, debug info

EcsDemoModule — small interactive demonstration (ASCII visualization)

These modules show how to extend NGINE-13G using the public EngineModule interface.

📂 Project Structure
src/
  engine.rs          // core loop + module management
  ecs.rs             // Entity, World, Components
  event.rs           // EventBus + events
  input.rs           // keyboard input utility
  main.rs            // entry point
  soul.rs            // placeholder for future expansion (unused here)

  modules/
    ecs_demo.rs
    heartbeat.rs
    stats.rs
    mod.rs

🛠️ Getting Started
Prerequisites

Rust (latest stable)

Cargo

Run the demo
cargo run

Adding your own module

Implement the EngineModule trait:

pub struct MyModule;

impl EngineModule for MyModule {
    fn name(&self) -> &'static str { "MyModule" }

    fn init(&mut self, events: &mut EventBus) {
        // Initialization logic
    }

    fn update(&mut self, frame: u64, dt: Duration, events: &mut EventBus, world: &mut World) {
        // Per-frame logic
    }
}


Then register it in main.rs:

engine.add_module(MyModule);


That’s it — NGINE-13G is fully modular.

🧭 Roadmap (Public)
Short-term

Additional built-in components

More visual demo modules

Better input/action abstraction

Mid-term

Plugin API stabilization

Optional rendering backend (TBD)

Serialization of world state

Long-term

Advanced simulation modules

Physics extensions

Scripting interface

🤝 Contributing

Contributions to NGINE-13G are welcome.
Please open an issue or pull request with proposed features, bug fixes, or improvements.

📄 License

This project uses the No License option.
You may fork and inspect the code, but re-use is not permitted without explicit written permission.

# Start Event
## Overview
The `StartEvent` is a event which is triggered when the engine starts, allowing for the execution of specific handlers that can perform necessary setup tasks, such as loading modules or initializing resources.

## Registering a `start_event` handler
This is a standard event meaning it follows the guidelines specified by the Event Standard.
This it can be registered like so:
```rust
RegisterEventHandler!(
        StartEventHandler,
        events::start_event::StartEvent,
        LibraryMetadata,
        |event: &mut events::start_event::StartEvent, mod_ctx: &Arc<LibraryMetadata>| {
            for n in event.modules.clone() {
                info!("Module: {:?}", n);
            }
            info!(
                "Event {:?} Handled by: {:?}, made by {}",
                event.id, &mod_ctx.mod_name, &mod_ctx.mod_author,
            );
        }
    );
    api.event_bus.event_handler_registry.register_handler(
            StartEventHandler { mod_ctx },
            ("core".to_string(), "start_event".to_string()),
        );
```

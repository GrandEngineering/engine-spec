# Events
## Event
An event is a unit of communication that encapsulates information about a specific occurrence within the application. Events are defined by implementing the `Event` trait, which requires several methods to be implemented:

- `clone_box`: Creates a boxed clone of the event.
- `cancel`: Marks the event as canceled.
- `is_cancelled`: Checks if the event is canceled.
- `get_id`: Returns the identifier of the event.
- `as_any` and `as_any_mut`: Provides type-safe access to the event's data.
## EventHandler

An event handler is responsible for processing events. Handlers are defined by implementing the `EventHandler` trait, which requires the `handle` method to be implemented. This method takes a mutable reference to an event and performs the necessary actions.

## EventCTX

The `EventCTX` trait extends `EventHandler` and provides additional context-specific handling capabilities. It includes methods to retrieve and handle events with specific types. Thus Downcasting the Generic Event into the correct Event.

## EventBus

The `EventBus` is the central component that manages the registration and dispatching of events and their handlers. It consists of two main registries:

- `EngineEventRegistry`: Stores registered events.
- `EngineEventHandlerRegistry`: Stores registered event handlers.

The `EventBus` provides methods to register events and handlers and to dispatch events to the appropriate handlers.

## Event Registration and Handling

### Registering Events

Events are registered using the `register_event` macro, which takes the API instance, module identifier, event name, and default state of the event as arguments. This macro simplifies the process of adding new events to the system.

### Registering Event Handlers

Event handlers are registered using the `RegisterEventHandler` macro. This macro defines a new handler struct and implements the necessary traits for handling the specified event type. Handlers can be registered with or without additional context. And to register the Handler you must use `api.event_bus.event_handler_registry.register_handler`.

### Handling Events

Events are dispatched and handled through the `EventBus`. When an event is dispatched, the `EventBus` looks up the registered handlers for the event's identifier and invokes their `handle` methods. Handlers can modify the event's state, cancel the event, or perform other actions based on the event's data.
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
This Example is executed upon program start and outputs all the loaded modules and their info.

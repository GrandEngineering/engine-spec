# Custom GRPC Event
## Introduction
The `CgrpcEvent` is a custom event type designed to handle gRPC (Google Remote Procedure Call) requests to the `Custom GRPC` Protocol within the. This event allows for the execution of specific handlers based on the provided handler identifier and payload. The event system is designed to be flexible and extensible, allowing for easy registration and handling of new event types.

By the creation of this event, we allow mods to be able to communicate with outside. For example authentication.

## Registering a `CgrpcEvent` Handler
To register a handler for a `CgrpcEvent`, you can use the `RegisterCgrpcEventHandler` macro. This macro simplifies the process of creating and registering event handlers.

Example usage:

```rust
RegisterCgrpcEventHandler!(cgrpcHandler, engine_core, grpc, |event: &mut CgrpcEvent| {
        event.output.write().unwrap().append(event.payload.as_mut());
        println!("grpc event!");
    });
api.event_bus.event_handler_registry.register_handler(
       cgrpcHandler {},
       ("core".to_string(), "cgrpc_event".to_string()),
   );
```
In this example:
- `MyHandler` is the name of the handler struct.
- `my_mod_id` and `my_handler_id` are the identifiers for the module and handler, respectively.
- The closure provided as the last argument contains the logic for handling the event.

## `zbus & Flying`

<br/><br/>
zeeshanak@gnome.org


Who am I?


Zeeshan Ali


Update on flying


Now a fixed-wing pilot


![](DA20.jpg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


Oxidising Geoclue


D-Bus


dbus-rs


D-Bus crate from scratch?? 😯


How hard can it be? 😂


Actually..really not too hard

Note: assumptions


zbus


Calling methods


GVariant


```rust
let mut conection = Connection::new_session().unwrap();
let reply = connection
    .call_method(
        Some("org.freedesktop.DBus"),
        "/org/freedesktop/DBus",
        Some("org.freedesktop.DBus.Peer"),
        "GetMachineId",
        None,
    )
    .unwrap();
let body = reply.body(Some(<&str>::SIGNATURE_STR)).unwrap();
let v = body.get(0).unwrap();
let id = v.get::<&str>().unwrap();
println!("Machine ID: {}", id);
```


Separate Variant crate


Receiving messages


Signals


Async


High-level API
<br/>
<br/>
Objects & Methods


Code generation


Maybe also macros


& way too much of easy!


That's all folks

<br/>
<br/>
https://gitlab.freedesktop.org/zeenix/zbus

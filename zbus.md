## `zbus`

<br/><br/>
Yet another D-Bus library

<br/><br/>
zeeshanak@gnome.org


Who am I?


Zeeshan Ali


![](redhat.png)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


FOSS


# 🛨  🚁  🐈


Background story


Geoclue


Geolocation D-Bus service


Written in C


Maintainer since Geoclue2


Why Rust?


Crash reports


Most sensitive data


gps-share


I <span style="color:red">❤️ </span> Rust


Only service code


Challenges


Meson


Maintainer wants a proper solution


Rust devs focused on web


custom_target()


```
cargo_script = find_program('cargo.py')
find = find_program('find')
c = run_command(find, 'src', '-name', '*.rs')
sources = c.stdout().strip().split('\n')
custom_target(
    'cargo-build',
    input: sources,
    output: [ 'geoclue' ],
    command: [
        cargo_script,
        ...
    ],
)
```


D-Bus


dbus-rs


Depends on libdbus


Multiple async APIs


No CI


Still decided to use


Even contributed


Rust GNOME Hackfest, May 2019


dbus-rs API over-complicated


D-Bus crate from scratch?? 😯


How hard can it be? 😂


Actually..really not too hard

Note: assumptions


zbus!


Established a connection!


Calling methods


GVariant


```rust
trait VariantType: Sized {
    const SIGNATURE: char;
    const ALIGNMENT: u32;

    fn encode(&self) -> Vec<u8>;

    fn extract_slice(data: &[u8], signature: &str)
        -> Result<&[u8], VariantError>;
    fn decode(bytes: &[u8], signature: &str)
        -> Result<Self, VariantError>;

    fn signature(&self) -> Cow<str>;
```


Basic types

<br/>
u8, i16, f64, str, etc


Container types


Arrays

<br/>
```rust
impl<T: VariantType> VariantType for Vec<T> {
...
```


Structures

<br/>
```rust
pub struct Structure(Vec<Variant>);

impl Structure {
    pub fn new(fields: Vec<Variant>) -> Self;
    pub fn fields(&self) -> &[Variant];
    pub fn take_fields(self) -> Vec<Variant>;
}

impl VariantType for Structure {
...
```


```rust
struct Variant {
    signature: String,
    value: Cow<[u8]>,
}
```


```rust
let i: i16 = 42;
let v = crate::Variant::from(i);
assert!(v.len() == 2);
assert!(v.is::<i16>());
assert!(v.get::<i16>().unwrap() == i);

let v = crate::Variant::from_data(
    v.bytes(),
    v.signature()
).unwrap();
assert!(v.len() == 2);
assert!(v.get::<i16>().unwrap() == i);
```


Fun with lifetimes

```rust
trait VariantType<'a>: Sized {
    const SIGNATURE: char;
    const ALIGNMENT: u32;

    fn encode(&self) -> Vec<u8>;

    fn extract_slice<'b>(data: &'b [u8], signature: &str)
        -> Result<&'b [u8], VariantError>;
    fn decode(bytes: &'a [u8], signature: &str)
        -> Result<Self, VariantError>;

    fn signature<'b>(&'b self) -> Cow<'b, str> {
        Cow::from(Self::SIGNATURE_STR)
    }
```


Back to D-Bus


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


Easy parts left


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

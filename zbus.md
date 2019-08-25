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


Conbributed


Rust GNOME Hackfest


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
    fn signature(&self) -> Cow<str> {
        Cow::from(Self::SIGNATURE_STR)
    }
}
```


Basic types

<br/>
u8, i16, f64, str, etc


Container types

<br/>
Structure & Array


```rust
struct Variant {
    signature: String,
    value: Cow<[u8]>,
}
```


```rust
let v = crate::Variant::from(i16::max_value());
assert!(v.len() == 2);
assert!(v.get::<i16>().unwrap() == i16::max_value());
assert!(v.is::<i16>());

let v = crate::Variant::from_data(v.bytes(), v.signature()).unwrap();
assert!(v.len() == 2);
assert!(v.get::<i16>().unwrap() == i16::max_value());
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
}
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

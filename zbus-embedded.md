![](zbus-logo.png)
<!-- .element style="border: 0; background: None; box-shadow: None" -->

Fearless IPC for embedded Linux is finally here!


Who am I?


Zeeshan Ali

<br/>
🇵🇰 🇫🇮 🇬🇧 🇸🇪  🇩🇪


![](lumeo.svg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


FOSS

<br/>
Desktop & Embedded


# 🛩  🚁  🐈


Background story


Oxidizing Geoclue


Geolocation D-Bus service


Written in C


What's D-Bus?? 🤔


Effecient binary IPC protocol


Desktop & embedded


systemd, GNOME & KDE etc


There must be a crate for it!


dbus-rs


libdbus 🙄


Issues

Note: Multiple async APIs, no CI etc


Still decided to use


Even contributed


Rust GNOME Hackfest, May 2019


dbus-rs API over-complicated


D-Bus crate from scratch?? 😯


How hard can it be? 😂


What's involved?


Low-level


Message passing


Wire format


<br/>
aka GVariant


Data types & encoding


Map nicely to Rust types


High-level


Objects

```
/org/freedesktop/GeoClue2/Manager
/org/freedesktop/GeoClue2/Client
/org/freedesktop/GeoClue2/Location/0
/org/freedesktop/GeoClue2/Location/1
...
```


Interfaces

```
org.freedesktop.GeoClue2.Manager
org.freedesktop.GeoClue2.Client
org.freedesktop.GeoClue2.Location
...
```


Methods

```
org.freedesktop.GeoClue2.Manager.GetClient(OUT o client)
org.freedesktop.GeoClue2.Client.Start()
org.freedesktop.GeoClue2.Client.Stop()
```


Signals

```
org.freedesktop.GeoClue2.Client.LocationUpdated(o old, o new)
```


Properties

```
org.freedesktop.GeoClue2.Location.Latitude
org.freedesktop.GeoClue2.Location.Longitude
org.freedesktop.GeoClue2.Location.Altitude
```


Actually..really not too hard

Note: assumptions


Cool, let's really do this!


zbus!


But first things first


zvariant


Spent several months


Different approaches


Fun with lifetimes


And D-Bus spec itself


zvariant 1.0


❌ Empty Arrays ❌


Why not serde?


Another few months of fun


zvariant 2.0


```rust
use zvariant::{from_slice, to_bytes, EncodingContext};

// All (de)serialization API needs a context.
let ctxt = EncodingContext::<byteorder::LE>::new_dbus(0);

let t = ("hello", 42i32, true);
let encoded = to_bytes(ctxt, &t)?;
let decoded: (&str, i32, bool) = from_slice(&encoded, ctxt)?;
assert_eq!(decoded, t);

//
```


Back to D-Bus


Slow progress


More D-Bus crates 😮


Help me out?


An old friend to the rescue


![](zeenix-elmarco-enterprise.jpg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->

Marc-André Lureau


Speed up


Lots of disagreements


Owned vs. Unowned


All essential API in place


How does it look like?


Low-level API


```rust
let mut connection = zbus::Connection::new_session()?;

let reply = connection
	.call_method(
			Some("org.gnome.SettingsDaemon.Power"),
			"/org/gnome/SettingsDaemon/Power",
			Some("org.gnome.SettingsDaemon.Power.Screen"),
			"StepUp",
			&(),
	)?;
 
let (percent, _)  = reply.body::<(u32, &str)>()?;
println!("New level: {}%", percent);
```


Pretty easy aleady


But we can do better


Hight-level API


Client-side

```rust
#[dbus_proxy]
trait Notifications {
    fn notify(&self,
              app_name: &str,
              replaces_id: u32,
              app_icon: &str,
              summary: &str,
              body: &str,
              actions: &[&str],
              hints: HashMap<&str, &Value>,
              expire_timeout: i32) -> zbus::Result<u32>;
}

//
```


```rust
let proxy = NotificationsProxy::new(&connection)?;

let _reply = proxy.notify(
	"my-app",
	0,
	"dialog-information",
	"Hi!!",
	"Yes, you! How are things?",
	&[],
	HashMap::new(),
	5000,
)?;
```


Server-side

```rust
#[dbus_interface(name = "org.zbus.MyGreeter1")]
impl Greeter {
    fn say_hello(&self, name: &str) -> String {
        format!("Hello {}!", name)
    }
}
```


```rust
use zbus::ObjectServer;

let mut server = ObjectServer::new(&connection);
server.at(&"/org/zbus/MyGreeter".try_into()?, Greeter);

loop {
	if let Err(err) = server.try_handle_next() {
		eprintln!("{}", err);
	}
}
```


Still not easy enough?


XML ↔ Code


Book

<BR/>
https://zeenix.pages.freedesktop.org/zbus


Release or didn't happen!!


🙌 1.0 🙌


Future plans

![](future.jpg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


GVariant


Async


Many other fixes & improvements


That's all folks

<br/>
<br/>
https://gitlab.freedesktop.org/zeenix/zbus

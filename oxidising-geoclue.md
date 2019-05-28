## `Oxididing Geoclue`

<br/><br/>
In which the protagonist takes an unexpected turn

<br/><br/>
zeeshanak@gnome.org


Who am I?


Zeeshan Ali


![](redhat.png)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


FOSS


# 🛨  🚁  🐈


What's Geoclue?


Finnish Conspiracy


Geolocation D-Bus service


Written in C


Maintainer since Geoclue2


Privacy


GNOME integration


Why Rust?


Crash reports


Most sensitive data


gps-share


I <span style="color:red">❤️ </span> Rust


Only service code


Challenges


Meson


Perfectionist maintainer


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


Easy parts left

..way too much of it!


That's all folks

<br/>
<br/>
https://gitlab.freedesktop.org/zeenix/zbus

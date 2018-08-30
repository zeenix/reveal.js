## `Fearless Multimedia Programming`

<br/><br/>
zeenix@collabora.com


Who am I?


Zeeshan Ali


![](collabora.png)
<!-- .element style="border: 0; background: None; box-shadow: None" -->

Collabora


FOSS


Flying & cats


What am I talking about?


Rust + GStreamer


Inspired by Sebastian Dröge's talk


Rust


Systems programming

<br/>
- Safety
- Efficiency


Zero-cost abstractions


No raw pointers

<br/>
- Deref dangling pointers <span style="color:red">x</span>
- Deref NULL pointers <span style="color:red">x</span>


`unsafe`

<br/>
FFI


Non-mutable state by default

<br/>
```
let x = 5;
let mut y = 5;
```


Strict ownership semantics

<br/>
Not a concept in C/C++


Garbage Collector

<br/>
in other languages


Only one owner

<br/>
```
let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

Note: basic types are Copy types


You can borrow

```
let s1 = String::from("hello");
let s2 = &s1;

println!("{}, world!", s1);
```
<br/>
Pass by reference in C++
<br/>


But borrows are temporary


`Rc<T>`

<br/>
```
let s1 = Rc::new(String::from("hello"));
let s2 = s1.clone();

println!("{}, world!", s1);
println!("{}, world!", s2);
```


Fearless concurrency


`Arc<T>`


`Mutex<T>`


![](gstreamer-logo.svg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


Multimedia


Pipelines (LEGO)

Insert pic here


Plugins


Written in C


Multithreaded

<br/>
- Apps
- Plugins


`OOP using GObject`


Why is Rust relevant?


Parsing media formats not safe

<br/>
Especially from untrusted sources

Note: FLV example


Multithreading is hard!


Mutability & ownership maps well

Note: GstMiniObject writable on refcount 1


Avoid memory errors

Note: A whole class of them


C an archaic language

Note: not only very unsafe


`GStreamer Rust bindings`

<br/>
- https://github.com/sdroege/gstreamer-rs
- https://github.com/sdroege/gst-plugin-rs


On a closing note...


No new projects in C/C++ please


That's all folks

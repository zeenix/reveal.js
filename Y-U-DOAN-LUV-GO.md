## `Y U DOAN LUV GO? 😾`

<br/><br/>
zeeshanak@gnome.org


Who am I?


Zeeshan Ali


![](redhat.png)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


FOSS


# 🛨  🚁  🐈


Disclaimer


❌ Bashing Go


Go evolves and improves


A major success story


Better than most languages out there


So why oh why I don't like it?


I don't **dislike** it :)


Quite a few things to admire


Super easy to learn


Async made simple


One of the best GCs


so again, what's my problem?


Some background


C, assembly & embedded


Intel 8051


![](KL_Intel_P8051.jpg)
<!-- .element style="border: 0; background: None; box-shadow: None" -->


Journey continued with C


Other languages along the way


Rust comes along


Why bother with Go?


Cloud-infra world


Get to it already!


Safety


1000 times safer than C


Comes at a cost


Null pointer


Lack of features


Enums


C

```c
typedef enum {
	CAR,
	MOTOR_BIKE,
} Vehicle;
```


Rust

```rust
enum Vehicle {
	Car(String),
	MotorCycle(registration: String),
}
```


C-like enum emulation


Generics


Not a concept in C


Rust

```rust
enum Option<T> {
    Some(T),
    None,
}
```


Code-generation in C & Go


Macros


C

```c
#define SIMPLE "hello"
#define SLIGHTLY_COMPLEX(x, y) (printf("%s %d", x, y))
```


Powerful beast in Rust

```rust
    // The type of `john` is `serde_json::Value`
    let john = json!({
        "name": "John Doe",
        "age": 43,
        "phones": [
            "+44 1234567",
            "+44 2345678"
        ]
    });

    println!("first phone number: {}", john["phones"][0]);

    // Convert to a string of JSON and print it out
    println!("{}", john.to_string());
```


Tooling


```
$ go get github.com/whatever
$
```


Build system


From autotools to meson


Cargo


Makefiles, shell scripts..


Why don't I just quit?


That's all folks

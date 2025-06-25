---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: ./components/blatt7.webp
# some information about your slides (markdown enabled)
title: Rust Structs \& Enums 
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Rust: Strukturen (Structs), Aufzaehlungen (Enums) und Pattern Matching

Alles kombinieren!

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>
---

# Uebersicht

<Toc text-sm minDepth="1" maxDepth="2" />


---
layout: image-right
image: ./components/blatt7.webp
---

# Rust - Definition von Enums 
Beispiel IP-Adressen
```rs 
enum IpAddrKind {
    V4, V6
}


fn main() {
    let four = IpAddrKind::V4; //kreiere Instanz.
    let six = IpAddrKind::V6; 
}

```


---
layout: image-right
image: ./components/blatt7.webp
---

# Rust - Definition von Enums 
Beispiel IP-Adressen
```rs 
enum IpAddrKind {
    V4, V6
}


fn main() {
    let four = IpAddrKind::V4; //kreiere Instanz.
    let six = IpAddrKind::V6; 
}

```

---
layout: image-right
image: ./components/blatt3.webp

---
# Rust - Definition von Enums in Structs 
Beispiel IP-Adressen
```rs 
enum IpAddrKind {
    V4,
    V6
}

struct IpAddr {
    kind: IpAddrKind,
    address: String,
}

fn main() {
    //initialisiere Struktur.
    let localhost = IpAddr { 
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1"); 
    }
}
```

---
layout: image-right
image: ./components/blatt3.webp
transition: slide-up
---

# Rust - Enums spezifizieren Werte
Beispiel IP-Adressen
```rs 
enum IpAddrKind {
    V4(String),
    V6(String)
}

//struct IpAddr {
//    kind: IpAddrKind,     
//    address: String,
//}
//^^ nicht mehr benoetigt. ^^

fn main() {
    //initialisiere Enum mit Wert.
    let localhost = IpAddrKind::V4(
            String::from("127.0.0.1")); 
}
```


---
layout: image-right
image: ./components/blatt3.webp

---

# Rust - Enums spezifizieren Werte **enhanced**
Beispiel IP-Adressen

```rs 
enum IpAddrKind {
    V4(u8,u8,u8,u8), //weitere Spezifikation.
    V6(u16,u16.. u16)
}
fn main() {
    //initialisiere IpV4 mit 4 unsigned short.
    let localhost = IpAddrKind::V4(
        127, 0, 0, 1); 
}
```

---

# Rust - Enums erlauben Methoden
Beispiel IP-Adressen

```rs 
enum IpAddrKind {
    V4(u8,u8,u8,u8), //weitere Spezifikation.
    V6(u16,u16.. u16)
}

impl IpAddrKind {
    pub fn doSomething(&self) -> i64 {
        // matching.. spaeter.
        if let &IpAddrKind::V4(i,j,k,m) = self { 
            ..
        } 
    }
    pub fn doSomethingElse(&self) -> String {
        ..
    }
}

fn main() {
    //initialisiere IpV4 mit 4 unsigned short.
    let localhost = IpAddrKind::V4(
        127, 0, 0, 1); 
    localhost.doSomething(); 
}
```
---

# Rust - Option \<T\> ersetzt *null*
Null gibt es in Rust nicht!
> demnach wird Option\<T\> eingefuehrt (mit T beliebiger Typ) Option\<T\> ist standardgemaeß mitdabei

````md magic-move

```rs 
//folgende Beispiel-Implementierung.
enum Option<T> {
    None,
    Some(T)
}
impl Option<T> {
    pub fn get .... 
}

fn main() {
    let x = None; 
    let mut y = Some(7); //Option<i64>
    y = x; //y is Null in that sense..
    println!("Got value {}", y.unwrap_or(1)); //ok! --> prints zero
}

```

```rs 
//folgende Beispiel-Implementierung.
enum Option<T> {
    None,
    Some(T)
}
impl Option<T> {
    pub fn get .... 
}

fn main() {
    let x: i8 = 5; 
    let y: Option<i8> = Some(5); 

    let sum = x+y //fail.
}
```
```rs 
//folgende Beispiel-Implementierung.
enum Option<T> {
    None,
    Some(T)
}
impl Option<T> {
    pub fn get .... 
}

fn main() {
    let x: i8 = 5; 
    let y: Option<i8> = Some(5); 

    //let sum = x+y //fail.
    let sum = x + y.unwrap_or(0);  //ok! --> '0' als Backup.

}
```
````

---
:

---

# Rust - *match* ist exhaustive

````md magic-move {lines: true}
```ts {all|1|2|3|4|9-11|12-14|15-18|all}
//typescript

enum Color {
    Red, 
    Green,
    Blue
}

function printColor(color: Color) {
    switch (color) {
        case Color.Red: 
            console.log('red'); 
            break; 
        case Color.Green: 
            console.log('green'); 
            break; 
        case Color.Blue: 
            console.log('blue'); 
            break; 
    }
}
```

```ts {*|7|21|*}
//typescript

enum Color {
    Red, 
    Green,
    Blue,
    Yellow
}

function printColor(color: Color) {
    switch (color) {
        case Color.Red: 
            console.log('red'); 
            break; 
        case Color.Green: 
            console.log('green'); 
            break; 
        case Color.Blue: 
            console.log('blue'); 
            break; 
        //Alles in Ordnung!
    }
}
```

```rs {3|4|5|6}
//rust

enum Color {
    Red, 
    Green,
    Blue,
}

fn print_color(color: Color) {
    match color {
        Color::Red => println!("red"); 
        Color::Green => println!("green"); 
        Color::Blue => println!("blue"); 
    }

}





```

```rs {all|7|15|all}
//rust

enum Color {
    Red, 
    Green,
    Blue,
    Yellow, //fuege "Yellow" hinzu
}

fn print_color(color: Color) {
    match color {
        Color::Red => println!("red"); 
        Color::Green => println!("green"); 
        Color::Blue => println!("blue"); 
        //Fehler hier.
    }

}


```

````

---

# Rust - Enums ermoeglichen Typsicherheit

````md magic-move
```ts [typescript-example.ts] {all|8|9|12|13|16|17|all} 
type Custom = {
    name: string,
    age: number
}
type Item = string | number | Custom;
const foo = "i am a string";

if(typeof foo === "string) { 
    console.log(foo); //ok!
} 

function append(arr: Item[]) {
    arr.push(84) 
}

const arr: string[] = ["hello", "world"];
append(arr);  //ok!



..

```

```rs [rust-example.rs] {all|15|16|19|21|all} 
struct Custom {
    name: String, 
    age: usize
}
enum Item {
    Foo(String),
    Bar(usize),
    Baz(Custom),
}

let foo = Item::Foo(String::from("hello")); 
if let Foo(s) = foo {
    println!("{}", s); //ok!    
}
fn append(arr: &mut Vec<Item>) {
    arr.push(Item::Bar(84)); 
}

let mut arr = vec!["hello", "world"];

append(&mut arr); //fail.
```
````

---


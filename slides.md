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

# Rust: Strukturen (Structs), Aufzählungen (Enums) und Pattern Matching

Motto: Kombinieren und Kreuzen!

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

# Übersicht

<Toc text-sm minDepth="1" maxDepth="1" />

---
transition: slide-up
---

# Rust - Strukturen

#### `struct` ist das Äquivalent zum `class`-keyword in C++

- erlaubt Werte verschiedener Typen 
- erlaubt Methoden
- erlaubt private und public
- Zugriff über keys 
- Instanziierung von Klassen -\> eröffnet OOP
- Jedoch keinen expliziten Konstruktor!

---
level: 2
---

# Rust - Strukturen - Initialisierung

````md magic-move
```rs
struct User {
    username: String,
    email: String,
    age: u32,
    active: bool
}

fn main() {
    let mut user1 = User {
        username: String::from("sandworm87"),
        email: String::from("gordonrampagedecreasedby21years@hotmail.com"),
        active: true,
        age: 38
    }
}

```

```rs
struct User {
    username: String,
    email: String,
    age: u32,
    active: bool
}

fn main() {
    user1 = build_user(String::from("foo@barMail.com", String::from("foobar");
}
//ähnlich zum Konstruktor, um Objekt zu bauen
fn build_user(email: String, username: String) -> User {
    User {
        email, 
        username,
        active: true,
        age: 38
    }
}
```

```rs
struct User {
    username: String,
    email: String,
    age: u32,
    active: bool
}

fn main() {
    user1 = build_user(String::from("foo@barMail.com", String::from("foobar");
    user2 = User { email: String::from("baz@pow.com", ..user1 } //übernehme Attribute user1 
    //-> Wiederbenutzung von Daten <=> Copy Constructor
}
//ähnlich zum Konstruktor, um Objekt zu bauen
fn build_user(email: String, username: String) -> User {
    User {
        email, 
        username,
        active: true,
        age: 38
    }
}
```
````

<!--
übernehme Attribute mit ..
-->

---
level: 2
---

# Rust - Zugriff auf Attribute von Strukturen (public und private)

````md magic-move
```rs
    User { //macht struct public
        username: String,
        age: u8,
    }
    fn main() {
        let user1 = User { username: String::from("foo"), age: 80 }
        user1.u8 //works
    }

```
```rs
    User { //macht struct public
        username: String, //in anderen Modulen nicht sichtbar.
        pub age: u8, //in anderen Modulen sichtbar!
    }
    fn main() {
        let user1 = User { username: String::from("foo"), age: 80 }
        user1.u8 //works
    }

```
````

<!--
public
-->

---
level: 2
---

# Rust - Methoden in Strukturen

( Referenzierung über '&self' ) 

````md magic-move
```rs
struct Rectangle {
    width: u32,
    height: u32
}
impl Rectangle {
    //wichtig self -> sonst ist Methode 'static'
    fn area(&self) -> u32 { //definiere Methode 'area'
        self.width * self.height 
    }
    fn can_hold(&self, other: &Rectangle) -> bool { //Methode can_hold
        self.width > other.width && self.height > other.height 
    }
}


```

```rs
struct Rectangle {
    width: u32,
    height: u32
}
impl Rectangle {
    fn area(&self) -> u32 { //definiere Methode 'area'
        self.width * self.height 
    }
    fn can_hold(&self, other: &Rectangle) -> bool { //Methode can_hold
        self.width > other.width && self.height > other.height 
    }
}


```
```rs
struct Rectangle {
    width: u32,
    height: u32
}
impl Rectangle {
    fn area(&self) -> u32 { //definiere Methode 'area'
        self.width * self.height 
    }
    fn can_hold(&self, other: &Rectangle) -> bool { //Methode can_hold
        self.width > other.width && self.height > other.height 
    }
} 
fn main() {
    let r0 = Rectangle { width: 8, height: 9 }; //erinnert an JSON
    println!("Rectangle r0 has the area: {}", r0.area());
    //prints 'Rectangle r0 has the area: 72'
}

```
```rs
struct Rectangle {
    width: u32,
    height: u32
}
impl Rectangle {
    fn area(&self) -> u32 { //definiere Methode 'area'
        self.width * self.height 
    }
    fn can_hold(&self, other: &Rectangle) -> bool { //Methode can_hold
        self.width > other.width && self.height > other.height 
    }
}
impl fmt::Display for Rectangle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "width is: {}, height is: {})", self.width, self.height)
    };
}

```
```rs
impl fmt::Display for Rectangle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "width is: {}, height is: {})", self.width, self.height)
    };
}
fn main() {
    let r0 = Rectangle { height: 8, width: 10 }; 
    println!("rectangle has the following dims: {}", r0); 
    //^^ prints 'rectangle has the following dims: width is: 10, height is 8'
}

```

````


---
level: 2
---

# Rust - Übergabe von Ownership der Strukturen (Zuweisung) 


````md magic-move
```rs
fn main() {
    let fruit1 = String::from("Banana"); // fruit1 is the owner of the String
    let fruit2 = fruit1; // ownership moves from fruit1 to fruit2

    // fruit1 is now invalid and cannot be used
    println!("fruit2 = {}", fruit2); // This works fine
}
```
```rs
//A pro pro Konstruktor 
impl Rectangle {
    pub fn new(width: u32, height: u32) -> Self { //static 
        //Self referenziert oberen Typ
       Self {
            width: width,
            height: height,
       }
    }
}
fn main() {
    let r0 = Rectangle::new(80, 100);  //Übergabe von Ownership
    r0.area();
}

```
```rs
//A pro pro Konstruktor 

fn main() {
    let r0 = Rectangle::new(80, 100);  //Übergabe von Ownership
    r0.area();
    let r1 = r0;  //Ownership nun bei r1
    r0.area(); //fail.

}

```
```rs
//Also.

fn main() {
    let r0 = Rectangle::new(80, 100);  //Übergabe von Ownership
    r0.area();
    let r1 = r0;  //Ownership nun bei r1
    r1.area(); //ok!

}

```

```rs 
struct Person {
    name: String,
    age: u8,
}
impl Person {
    fn greet(&self) {
        println!("no way you found me!")
    }
}


fn main() {
    let p = Person { name: String::from("Kelvin Klein"), age: 80};
    let p1 = p;
    p.greet(); //fail.
}
```

````

---
level: 2
---

# Rust - *Tupel*-Strukturen *Tuple-Structs*

eine Art *named* Tupel, jedoch auf den Typ reduziert
```rs
struct Point2d(u32, u32); 

struct Point3d(i32, i32, i32); 
struct Color(i32, i32, i32); 

fn do_something(p: Point3d) {
    //do_something with point
}

fn main() {
    let foo = Color(8, 9, 255); 
    do_something(foo); //fail.
}

```

- alle Attribute sind per se privat für andere Module
- Attribute haben keinen Namen -\> Zugriff über Index e.g. `p.0`, `p.1`
- dienen der Unterscheidung im Code - erhöht Übersicht
- können Methoden haben


---
level: 2
---

# Rust - *Unit*-Strukturen (*Unit-Structs*)

```rs
// Define a unit struct
struct UnitStruct;

// Define a trait with a method `describe`
trait Describable {
    fn describe(&self) -> String;
}

// Implement the Describable trait for UnitStruct
impl Describable for UnitStruct {
    fn describe(&self) -> String {
        "This is a unit struct implementing the Describable trait.".to_string()
    }
}
```
- eine Art abstrakte Klasse 
- bzw. eine Namespace `UnitStruct`

---
transition: slide-up
level: 1
---

# Rust - Enums (in C)

```c 
enum Zahl = { EINS=1, ZWEI=2, DREI=3 }; //nicht die funktionalste Form für Enums 
```
 


---
layout: image-right
image: ./components/blatt7.webp
level: 2
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
level: 2
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
level: 2
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
//^^ nicht mehr benötigt. ^^

fn main() {
    //initialisiere Enum mit Wert.
    let localhost = IpAddrKind::V4(
            String::from("127.0.0.1")); 
}
```

---
level: 2
---

# Rust - Enums spezifizieren Werte **enhanced**

````md magic-move
```rs 
// Beispiel IP-Adressen
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

```rs 
// Beispiel Haus
enum Gestein ..
enum Holz {
    Kiefer,
    Fichte,
    Spahn,
}
enum Metall {
    Stahl,
    Aluminium,
    Gold
}
enum Haus<T,U> {
    Stelzen(T, T, U, U), //weitere Spezifikation.
    Dach(U, T),
    Mauer
}
fn main() {
    let hausStelzen = Haus::Stelzen(
        Holz::Kiefer, Holz::Kiefer, //2x Holz
        Metall::Aluminium, Metall::Stahl); //2x Metall
    let hausDach = Haus::Dach(Metall::Stahl, Gestein::Backstein); 
} //es geht noch komplizierter in dem struct für U oder T einsetzt
```
```rs 
// Beispiel Haus
....
struct Material<W> {
    art: W,
    scoreBeschaffenheit: u8, //score
}
enum Kosten {
    HOCH, 
    MITTEL,
    NIEDRIG
}
enum Haus<T,U> {
    .. ..
    Dach(U, T),
}
fn main() {
    let hausDach = Haus::Dach(
        Kosten::HOCH, 
        Haus {
            art: Metall::Gold,
            scoreBeschaffenheit: 200,            
        }
    ); 
} 
```

````


---
level: 2
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
        // matching.. später.
        if let &IpAddrKind::V4(i,j,k,m) = self { 
            //do something..
        } 
    }
    pub fn doSomethingElse(&self) -> String {
        //do something else..
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
transition: slide-up
level: 2
---

# Rust - Option \<T\> ersetzt *null* 
Null gibt es in Rust nicht!
> demnach wird Option\<T\> eingeführt (mit T beliebiger Typ) Option\<T\> ist standardgemäß mitdabei

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
level: 1
---

# Rust: Was tun mit Enums? -> Matching!

````md magic-move
```rs 
enum Muenze {
    EinCent,
    FuenfCent, 
    ZehnCent, 
    FuenfzigCent 
}

fn value_in_cents(coin: Muenze) -> u8 {
    match coin { //match
        Muenze::EinCent => 1,
        Muenze::FuenfCent => 5,
        Muenze::ZehnCent => 10,
        Muenze:: FuenfzigCent => 50
    }
}

```

```rs 
enum Standort {
    USA,
    Deutschland
}

enum Muenze {
    EinCent,
    ZehnCent, 
    FuenfzigCent(Standort)
}

fn value_in_cents(coin: Muenze) -> u8 {
    match coin { //match
        Muenze::EinCent => {
            println!("Hey, ein Cent!"); 
            1
        },
        Muenze::ZehnCent => 10,
        Muenze:: FuenfzigCent(state) => {
            println!("Fuenzig Cent aus {:?}!", Standort);
            50
        }
    }
}

```
```rs 


fn plus_one(x: Option<i32>) -> Option<String> {
    match x {
        None => None, //Pflicht
        Some(i) => Some((i+1).to_string())
    } 
}

```

```rs 
//alternativ dazu mit '_'

fn plus_one(x: Option<i32>) -> Option<String> {
    match x {
        _ => (), //_ für den sonstigen Fall - gibt None zurück
        Some(i) => Some((i+1).to_string())
    } 
}

```
````

---
level: 2
---

# Rust - *match* ist **exhaustive** und 'if let'..


<table>
<tr>
<td style="width: 400px;">
````md magic-move {lines: true}
```ts {all|1|2|3|4|11-13|14-16|all}
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
````

</td>
<td style="width: calc(800px-50%);">

````md magic-move {lines: true}
```rs {4|5|6|7}
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
    Yellow, //füge "Yellow" hinzu
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

```rs {all|7|15|all}
//alternativ: if let Syntax umgeht match-Requirement
enum Brightness {
    Light,
    Dark
}

enum Color {
    Red, 
    Green,
    Blue(Brightness)
}

fn print_color(color: Color) {
    if let Red = color {
        println!("I only match red!"); 
    } else {
        println!("Did not match red!"); 
    }
}


```

```rs {all|7|15|all}
//alternativ: if let Syntax umgeht match-Requirement
enum Brightness {
    Light,
    Dark
}

enum Color {
    Red, 
    Green,
    Blue(Brightness)
}

fn print_color(color: Color) {
    if let Blue(Brightness::Light) = color {
        println!("It's light blue!"); 
    } 
}


```
````
</td>
</tr>
</table>


---
level: 2
---

# Rust - Enums ermöglichen Typsicherheit


<table>
<tr>
<td style="width: 400px;">
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
</td>
<td style="width: calc(800px-50%);">

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
</td>
</tr>
</table>

---
layout: center
---

# Fragen?

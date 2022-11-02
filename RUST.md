# Inhalt

[[RUST#Installation (unter Windows)]]
[[RUST#Projekt anlegen]]
[[RUST#STDOUT, STDERROR and format]]
[[RUST#STDIN]]
[[RUST#Numerische Datentypen]]
[[RUST#Castings]]
[[RUST#Arrays]]
[[RUST#Nützliches zu Arrays]]
[[RUST#Generics]]
[[RUST#Vektor]]
[[RUST#String]]
[[RUST#Tupel]]
[[RUST#Schleifen]]
[[RUST#MATCH]]
[[RUST#IF LET]]
[[RUST#WHILE LET]]
[[RUST#Functions]]
[[RUST#Closure & Function Pointer]]
[[RUST#Structs]]
[[RUST#Traits]]
[[RUST#Konstanten -> Const]]
[[RUST#ENUMS]]
[[RUST#Ownership]]
[[RUST#BOX Heap Pointer]]
[[RUST#Reference Counter RC]]
[[RUST#Lifetime Annotations]]
[[RUST#Standard Librarys]]
[[RUST#ERROR Handling]]
[[RUST#Arguments bzw. Parameter in Konsolenanwedung übergeben]]




# Installation (unter Windows)

Erst müssen die Microsoft Bulid Tools für C++ installiert werden (Achtung sind über 4 GB)
https://visualstudio.microsoft.com/de/visual-cpp-build-tools/

https://www.rust-lang.org/tools/install
Hier Prüfen ob unter `default host triple:` der Compiler richtig angegeben ist. z.B.: `x86_64-pc-windows-msvc`

Dann starten mit  Option 1.

Prüfen ob Installation erfolgreich war:
```cmd
rustc --help
```

Wenn hier eine ausgabe der Optionen erfolgt ist die PATH Variable angelegt und mit großer wahrscheinlichkeit RUST richtig installiert. -> Wenn nicht hilft evtl. ein Neustart.

[[RUST#Inhalt]]

# Projekt anlegen

Mit `cargo` werden neue Projekte angelegt:
Namenskonvention ist Snake-Case also z.B.: `hello_world`

```cmd
cargo new {NAME}
```

Version kontrollieren:
```cmd
rustc --version
```

Projekt kompilieren:
```cmd
cd {Projektordner}
cargo build
```

Code kompilieren und anschließend ausführen:
```cmd
cd {Projektordner}
cargo run
```

Externe Pakete:
https://crates.io/

einbinden von Paketen in der Cargo.toml (in diesem Bsp.: "rand"):
```yaml
[dependencies]
rand = "0.8.5"
```

anschießend in der `*.rs`:

```rust
use rand::*; // Alles verwenden

// oder

use rand::Rng; // Nur Rng verwenden
```

[[RUST#Inhalt]]

# STDOUT, STDERROR and format

```rust
fn main() {
	let name = "Bob";
	let sentence = format!("Hallo {}", name);
	//new Feature
	let newsentnece = format!("Hallo {name}");

    //Print STDOUT
	print!("{}", name);
	println!("{}", name);

    println!("{}", sentence);

	//STD Error
	eprint!("{}", name);
	eprintln!("{}", sentence);
}
```

[[RUST#Inhalt]]

# STDIN

```rust
use std::io;
  
fn main() {
    let mut number = String::new();  

    io::stdin()
        .read_line(&mut number)
        .expect("Failed to read number!"); 

    println!("{}", number);
}
```

[[RUST#Inhalt]]

# Numerische Datentypen

`std::primitive`

Doku:
https://doc.rust-lang.org/std/primitive/index.html

Standard:
```rust
let test = 0; //Per Default 32Bit Signed Integer

let testFloat = 2.0; // Per Default 64Bit Floating Point
```

Explixit:
```rust
let test: u8 = 0; // Unsigned 8Bit Integer
let test: f32 = 0.0; // 32 Bit Float
let test: i8 = 0; // Signed 8Bit Integer
```

```rust
i218::MAX;
u8::MAX;
u64::MIN;
```

Zuweisung in Dezimal, Hex und Binär:
```rust
let test1 = 255; // Dezimal
let test2 = 0xff; // Hexadezimal
let test3 = 0b11111111; // Binär
```

[[RUST#Inhalt]]

# Castings

Doku:
https://doc.rust-lang.org/rust-by-example/types/cast.html


```rust

let integer_test: u32 = 62000;

let byte_test: u8 = integer_test as u8;
```

[[RUST#Inhalt]]

# Arrays

Doku:
https://doc.rust-lang.org/std/primitive.array.html

```rust
let test_Array = [1,2,3,4,5]; // Signed 32 Integer
```

Array per Index ausgeben:
```rust
fn main() {
	let test_Array = [12,24,56];

	println!("{:?}", test_Array[1])
}
```

Array mit format! Makro ausgeben:
```rust
fn main() {
	let test_Array = [12,24,56,42];

	println!("{:?}", test_Array)
}
```

Auschnitt aus Array:
```rust
println!("Slice: {:?}", &test_Array[1..4]) //Exclusiv letzer Zahl

println!("Slice: {:?}", &test_Array[1..=3]) //inklusiv letzer Zahl
```


## Nützliches zu Arrays
`std::fmt`
https://doc.rust-lang.org/std/fmt/index.html

Legt ein Array mit 10 Speicherplätzen vom Typ unsigned 8Bit Integer an 
und weist allen 10 die Zahl 245 zu.
```rust
let test_Array: [u8; 10] = [245; 10];
```

Länge Array ausgeben:
```rust
println!("Length: {}", test_Array.len());
```

Speicherbedarf ausgeben:
```rust
use std::mem;

println!("Size in Memory: {}", mem::size_of_value(&test_Array));
```

[[RUST#Inhalt]]

# Generics

```rust
struct Newtype<T> {
    item: T
}  

fn main() {
    let _new_item = Newtype {item: String::from("test")};
    let _new_item2 = Newtype {item: 32};
}
```

## Wichtige Traits

```rust
use std::fmt::Debug;
use std::fmt::Display;

impl<T> Newtype<T>
where
	T: Debug + Display,
{
	fn do_something(&self) {
		println!("{}", self.item);
	}
}
```

[[RUST#Inhalt]]

# Vektor

`Vec<T>` Ist vergleichbar mit `List<T>` aus C#.

[[RUST#Inhalt]]

# Iter

```rust
// by Value Iteration
let items = TypeOf.into_iter().next().unwrap();

// by Reference
let mut items = TypeOf.iter(); // => (&TypeOf).into_iter();
let first_item &TypeOf = items.next().unwrap()

// by mutable Reference
let mut items = TypeOf.iter_mut(); // => (&mut TypeOf).into_iter();
let first_item &mut TypeOf = items.next().unwrap()


```

[[RUST#Inhalt]]

# String

`std::str`
`std::string::String`

Doku:
https://doc.rust-lang.org/std/string/struct.String.html

```rust
let test_Str = "Hello World"; // str => string slice

let test_String = String::from("Hello World"); // Unmutable 
let mut test_String_2 = String::from("Name"); // Mutable

test_String.push('T'); // Error Variable ist Unmutable
test_String_2.push('T'); // Kein Fehler Variable ist Mutable!
```

Index auf Chars innerhalb Strings nicht möglich, Rust zeigt auf Bytes.
```rust
println!("{}", test_String[0]); // Error dieser Zugriff auf Chars per Integer ist nicht moeglich!

// Das wuerde funktionieren
for i in test_String.bytes() {
	println!("{}", i);
}

// Das entspicht mehr dem bekannten Muster 😉
for c in test_String.chars() {
	println!("{}", c);
}

```

Casten von Strings (Bespiel):
Mit `.trim()` werden Leerzeichen entfernt. 
Mit `.parse()::<u32>() {}` wird gecastet. Die Angabe `<TYPE>` definiert den Typ in den Geparst wird.
```rust
let number = String::from("   34 "); 

let string_as_number = match number.trim().parse::<u32>() {
	Ok(num) => num,
    Err(_) => 0,
};
```

[[RUST#Inhalt]]

# Tupel

Doku:
https://doc.rust-lang.org/std/primitive.tuple.html

```rust
let tup = (500, "TEST", true, 9.0);
```

Indexieren:
```rust
tup.[NUMMER]

tup.1 // Zugriff per index auf inhalt => startet bei index 0

// oder

let (a,b,c,c) = tup;
```

[[RUST#Inhalt]]

# Schleifen

## LOOP

Doku:
https://doc.rust-lang.org/std/keyword.loop.html

Ende des loop's per `break`. 

```rust
let mut counter = 0;

loop {
	counter += 1;
	// Do Something

	if counter == 10 {
		break;
	}
}
```

Loops können auch Namen gegeben werden:

```rust
let mut counter = 0;

'nameOfLoop: loop {
	counter += 1;
	// Do Something

	if counter == 10 {
		break 'nameOfLoop;
	}
}
```

[[RUST#Inhalt]]

## WHILE

Doku:
https://doc.rust-lang.org/std/keyword.while.html

```rust
let mut counter = 0;

while counter < 10 {
	counter += 1;
	// Do Something
}

// Oder Auch wie bei Loops mit Namen
let mut counter = 0;

'while_name: while counter < 10 {
	counter += 1;
	// Do Something
}
```

[[RUST#Inhalt]]

## FOR

Doku:
https://doc.rust-lang.org/std/keyword.for.html

```rust
for i in variable {
	...
}

// Über Iterierbare Variablen wie z.B.: Arrays
let count = 0..11;

for item in count {
	// Do Something with item
}
```

Mit Iterator `iter`:
`(i, &test)` stellen einen Tupel dar.
Die `string` Variable wird erst in ein byte `.as_bytes()` gewandelt und anschließend mit `.iter().enumerate()` Enumerierbar gemacht.

```rust
let hello = String::from("Hello World");

for (i, &test) in hello.as_bytes().iter().enumerate(){
        println!("{:?}",&test);
    }
```

Diese variante generiert aber einen Kompilerwarnung da `i` nicht verwendet wird. Das könnte mit `_i` verhindert werden.

[[RUST#Inhalt]]

# MATCH

Doku:
https://doc.rust-lang.org/std/keyword.match.html

Vergleichbar mit `switch` in C#

```rust
let number = 58;

match number {
	1 => println!("FALSCH"), // Match ist 1
	58 => println!("RICHTIG!"),
	3 => {
		println!("AUCH SOWAS"); // Mit Anweisungsblock {} moeglich
		println!("geht");
	}
	_ => println!("WEIS NICHT")	// Alles andere
}

```

[[RUST#Inhalt]]


# IF LET

Doku:
https://doc.rust-lang.org/rust-by-example/flow_control/if_let.html

[[RUST#Inhalt]]


# WHILE LET

Doku:
https://doc.rust-lang.org/rust-by-example/flow_control/while_let.html

`into_iter()` => https://doc.rust-lang.org/std/iter/trait.IntoIterator.html

```rust
fn iterate_through_nums() {
    let mut nums = (0..11).into_iter();  

    while let Some(num) = nums.next() {
        println!("{}", num);
    }
}

fn main() {
    iterate_through_nums();
}
```

[[RUST#Inhalt]]

# Functions

Doku:
https://doc.rust-lang.org/book/ch03-03-how-functions-work.html

Start der Funktion immer mit keyword `fn`.
Parameter werden so beschrieben `[NAME_VARAIBLE]: [TYPE]` z.B.: `test: String`
Rückgabewert wird mit einem Lambdaausdruck `->` und dem Typen angegben z.B.: `-> f64` 
Return innerhalb Funktion wie im unteren Bsp. (Kein Semikolon am Ende der Zeile -> RETURN)

```rust
fn my_test_func(x: i32) -> i32 {
	 x * x
}

// Auch möglich
fn my_test_func(x: i32) -> i32 {
	return x * x;
}

// Oder ohne Semikolon
fn my_test_func(x: i32) -> i32 {
	return x * x
}

```

[[RUST#Inhalt]]

# Closure & Function Pointer

Doku:
https://doc.rust-lang.org/book/ch13-01-closures.html
https://doc.rust-lang.org/rust-by-example/fn/closures.html
https://doc.rust-lang.org/reference/types/closure.html

Die Parameter kommen zwischen die zwei `||` der Rückgabewert wird mit einem `->` gestartet und mit dem Typen beendet. Die Operation kommt zwischen `{ }`.
Vergleichbar einer Lambda Function von C#.

```rust
// Simple Block
let y: i32 = 5;
let addition = |x: i32| x + y;
println!("{}", addition(5));

// weitere Möglichkeit (like an Function Pointer)
let closureTest = |x: i32| -> i32 {x * x};
let erg = closureTest(36);

// if single line Expression
let closureTest = |x: i32| x * x;

// noch kleiner
let closureTest = |x| x * x;


// Function Pointer
fn add (x: i32, y: i32) -> i32 {x +y}

fn calc_and_print(x: i32, y: i32, calculator: fn(i32, i32) -> i32) {
	let result = calculator(x,y);
	println!("{}", result);
}

calc_and_print(1,3,add);
calc_and_print(1,3,|x, y| x + y); // Or by closure

// Extern variable
// + '_ ist eine Lifetime Annotation, ohne => Compiler error!
fn calc_and_print(x: i32, y: i32, calculator:Box<dyn Fn(i32, i32) -> i32 + '_>) {
	let result = calculator(x,y);
	println!("{}", result);
}

let z = 5;
calc_and_print(1,3, Box::new(|x,y| x + y + z));

// Funktion als T
fn math<T>(x: i32, y: i32, calculator: T) -> i32
    where
        T: Fn(i32, i32) -> i32,
{
	calculator(x,y)
}

let subtr = |x,y| x-y;
math(10,5, subtr);
```

[[RUST#Inhalt]]

# Structs

Doku:
https://doc.rust-lang.org/std/keyword.struct.html

Um ein Struct auszugeben muss `#[derive(Debug)]` angegeben werden.

```rust
#[derive(Debug)]
struct Circle {
	radius: f64,
	location_x: f64,
	location_y: f64,
}

fn main(){
	let new_circ = Circle {
	    radius: 25.0,
        location_x: 10.0,
        location_y: 298.0,
	 };
    println!("{:?}", new_circ);
}
```

## Struct Functions

mit `impl` wird die Funktion dem gleichbenannten Struct zugeordnet. Mit `&self` werden auf die Member zugegriffen entrspricht z.B.: `this` in C#. Soll im Parameter noch ein gleicher Struct Typ mit übergeben werden kann `&Self` (Großes S anstatt s) verwendet werden. z.B.: bei der Implementierung eines Vergleichers.
Die allgemeine Schreibweise von Funktionen ist der Lambda Ausdruck:

`fn name_funktion(PARAMETER) -> RUECKGABEWERT {}`

```rust
#[derive(Debug)]
struct Circle {
	radius: f64,
	location_x: f64,
	location_y: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        self.radius * self.radius * std::f64::consts::PI
    }

	fn is_bigger(&self, other: &Self) -> bool {
		self.radius > other.radius
	}
}

fn main(){
	let new_circ = Circle {
	    radius: 25.0,
        location_x: 10.0,
        location_y: 298.0,
	};

	let new_circ_2 = Circle {
	    radius: 12.0,
        location_x: 10.0,
        location_y: 298.0,
	};
	 
    println!("{:?}", new_circ);
    
    println!("{:?}", new_circ.area());
	
	println!("{:?}", new_circ.is_bigger(&new_circ_2));
}
```

[[RUST#Inhalt]]

# Traits

Doku:
https://doc.rust-lang.org/book/ch10-02-traits.html
https://doc.rust-lang.org/rust-by-example/trait.html

Ein Trait in rust ist vergleichbar eines Interfaces in C# => es ist ein Contract.

```rust
trait Area {
    fn calculate_area(&self) -> f64;
}

struct Rectangle {
    length: f64,
    wide: f64
}

struct Circle {
    radius: f64
}

impl Area for Rectangle {
    fn calculate_area(&self) -> f64 {
        self.length * self.wide
    }
}
  

impl Area for Circle {
    fn calculate_area(&self) -> f64 {
        self.radius * self.radius * std::f64::consts::PI
    }
}
  
fn main() {
    let circle1 = Circle {radius: 125.12};
    let rect1 = Rectangle {length: 1256.4, wide: 145.9};  

    println!("{}",circle1.calculate_area());
    println!("{}",rect1.calculate_area());
}
```

Noch viel eleganter 😀:
Es kann eine Funktion erstellt werden die wiederum die Funktion aus dem Trait implementiert. 
D.H.: alle structs die Traitfunktion `calculate_area` implementieren können an die Funktion `do_calculate` übergeben werden.
```rust
trait Area {
    fn calculate_area(&self) -> f64;
}

struct Rectangle {
    length: f64,
    wide: f64
}  

struct Circle {
    radius: f64
}
  
impl Area for Rectangle {
    fn calculate_area(&self) -> f64 {
        self.length * self.wide
    }
}

impl Area for Circle {
    fn calculate_area(&self) -> f64 {
        self.radius * self.radius * std::f64::consts::PI
    }
}

// Hier eine Funtion die das Tait implemntiert
fn do_calculate(element: &impl Area) {
    println!("{}", element.calculate_area());
}  

fn main() {
    let circle1 = Circle {radius: 125.12};
    let rect1 = Rectangle {length: 1256.4, wide: 145.9};  

    do_calculate(&circle1);
    do_calculate(&rect1);
}
```

[[RUST#Inhalt]]

# Konstanten -> Const

Alles im Variablennahmen wird in Großbuchstaben geschrieben.

```rust
const DASISTEINEKONSTANTE: i32 = 15;
```

[[RUST#Inhalt]]

# ENUMS

Doku:
https://doc.rust-lang.org/std/keyword.enum.html

In Rust sind Funktionen im Enum möglich 😁:

```rust
enum DayOfWeek {
	Monday,
	Tuesday,
	Wednsday,
	Thursday,
	Friday,
	Saturday,
	Sunday,	
}

impl DayOfWeek {
	fn number_of_day(&self) -> u8 {
		match self {
			DayOfWeek::Monday => 1,
			DayOfWeek::Tuesday => 2,
            DayOfWeek::Wednsday => 3,
            DayOfWeek::Thursday => 4,
            DayOfWeek::Friday => 5,
            DayOfWeek::Saturday => 6,
            DayOfWeek::Sunday => 7
		}
	}
}

let day = DayOfWeek::Monday;

println!("{}", day.number_of_day());
```

Ein einzelner Enum Wert kann verschiedene Datentypen annehmen 😖:

```rust
enum User {
	None,
	Username(String),
    Userage(u16),
}

let us = User::Username(String::from("BOB")); 

let age = User::Userage(16);
```

Spezielle Anwedung:
```rust
fn addition(x: u32, y: Option<u32>) -> u32) {
	match y {
		Some(val) => x + val,
		None => x,
	}
}

fn main(){
	let y: Option<u32> = Some(67);

	println!("{}", addition(20, y));
}
```

Mehr Info zu `Option<T>`:

Doku:
https://doc.rust-lang.org/std/option/index.html

[[RUST#Inhalt]]


# Ownership

Doku:
https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html

Beispiel move || :
Hier erklärt von Rainer Stropek: https://www.youtube.com/watch?v=bgZa9VRBhYU
```rust
use std::time::Duration;
use std::thread;

fn main() {
    let mut counter = 0;
    let mut counter_string = String::new();
  
    // move Ownership der Variablen in den thread
    let backgound = thread::spawn(move || {
        loop {
            counter += 1;  

            if !counter_string.is_empty() {
                counter_string.push_str(", ");
            }  

            counter_string.push_str(&counter.to_string()); 

            println!("{}", counter_string);  

            if counter == 5 {
                break;
            }  

            thread::sleep(Duration::from_millis(100));
        }
    });  

    backgound.join().unwrap();
}
```

[[RUST#Inhalt]]

# BOX Heap Pointer

Doku:
https://doc.rust-lang.org/book/ch15-01-box.html?highlight=box%3CT%3E#using-a-box-to-store-data-on-the-heap
https://doc.rust-lang.org/beta/std/boxed/struct.Box.html

Legt den Typen im HEAP ab:
```rust
#[derive(Debug)]
struct Bike {
    e_bike: bool,
}

fn main() {
    let bike1 = Box::new(Bike{e_bike: true});
    println!("{:#?}", bike1);
}
```

[[RUST#Inhalt]]

# Reference Counter RC

Doku:
https://doc.rust-lang.org/beta/std/rc/struct.Rc.html
https://doc.rust-lang.org/book/ch15-04-rc.html

Pointer auf Speicher der mehrfach verwendet werden kann (Ownership bei Rust).

Problem:
```rust
#[derive(Debug)]
struct Bike {
    e_bike: bool,
}

fn main() {
    let bike1 = Bike{e_bike: true};
    let bike2 = bike1; // Move Ownership 😑

    println!("{:#?}", bike1);
}
```

Loesung mit RC:
```rust
use std::rc::Rc; 

#[derive(Debug)]
struct Bike {
    e_bike: bool,
}

fn main() {
    let bike1 = Rc::new(Bike{e_bike: true});
    let bike2 = bike1.clone(); // Clone Pointer 😁   

    println!("{:#?}", bike1);
}
```

[[RUST#Inhalt]]

# Lifetime Annotations

Doku:
https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html

Problem:
Compiliert nicht da der Zeiger bzw. die Referenz der Rückgabe der Funktion auf einen ungültigen Speicherplatz zeigt. Out of scope 😭
```rust
#[derive(Debug)]
struct Bike {
    e_bike: bool,
    size: u32,
}

fn compare_bikes (x: &Bike, y: &Bike) -> &Bike {
    if x.size > y.size {
        x
    } else {
        y
    }
}
  
fn main() {
    let bike1 = Bike{e_bike: true, size: 10};
    let bike2 = Bike{e_bike: false, size: 15};  
   
    let result = compare_bikes(&bike1, &bike2);  

    println!("{:#?}", result)
}
```

Lösung: 
Verwenden von Lifetime Annotations. Gibt der Referenz eine Lifetime über den Scope hinaus. -> `'a`
```rust
#[derive(Debug)]
struct Bike {
    e_bike: bool,
    size: u32,
}

fn compare_bikes<'a> (x: &'a Bike, y: &'a Bike) -> &'a Bike {
    if x.size > y.size {
        x
    } else {
        y
    }
}

fn main() {
    let bike1 = Bike{e_bike: true, size: 10};
    let bike2 = Bike{e_bike: false, size: 15};  

    let result = compare_bikes(&bike1, &bike2);

    println!("{:#?}", result)
}
```

[[RUST#Inhalt]]

# ERROR Handling

Doku:
https://doc.rust-lang.org/rust-by-example/error.html

Beispiel für das öffnen eines Files, mit `panic!()` wird das Progamm hier abgebrochen.

```rust
let f = File::open("test.txt");

let f = match f {
	Ok(file) => file,
	Err(error) => match error.kind() {
		ErrorKind::NotFound => panic!("File not found!"),
		_ => panic!("Unknown error!"),
	};
}

// Option Some und None
// => https://doc.rust-lang.org/std/option/enum.Option.html
fn function_opt(x: i32) -> Option<i32> {
	if x >= 0 {
		return some(val + 23);
	}
	None
}

// Result Ok and Err
// => https://doc.rust-lang.org/std/result/enum.Result.html
fn function_result(x: i32) -> Result<i32, String> {
	if x >= 0 {
		return Ok(val + 26);
	}
	Err(String::from("Error Message"))
}

// Auswertung Enum Some None
// => https://doc.rust-lang.org/std/option/enum.Option.html
let ret_inp = match function_opt(1) {
	Some(value) => value,
	None => panic!("Error")
};

// Auswertung Enum Ok Err
// => https://doc.rust-lang.org/std/result/enum.Result.html
let ret_inp2 = match function_result(2) {
	Ok(value) => value,
	Err(err) => panic!("Error")
};

// Mit Panic
let exp = function_result(7).expect("Error message");
// oder kurz (unwrap wirft panic)
let wrap = function_opt(inp).unwrap();

```

[[RUST#Inhalt]]

# Standard Librarys

`std::fs` -> Filesystem
https://doc.rust-lang.org/std/fs/
https://doc.rust-lang.org/rust-by-example/std_misc/fs.html

`std::env` -> Environment
https://doc.rust-lang.org/std/env/index.html
https://doc.rust-lang.org/book/ch12-05-working-with-environment-variables.html

`std::io::{Error, ErrorKind`} -> Fehlerunterstuetzung


[[RUST#Inhalt]]

# Arguments bzw. Parameter in Konsolenanwedung übergeben

```rust
use std::env;

fn main() {
    let arg: Vec<String> = env::args().collect();
}
```

[[RUST#Inhalt]]


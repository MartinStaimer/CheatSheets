
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

# Castings

Doku:
https://doc.rust-lang.org/rust-by-example/types/cast.html


```rust

let integer_test: u32 = 62000;

let byte_test: u8 = integer_test as u8;
```
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


# IF LET

Doku:
https://doc.rust-lang.org/rust-by-example/flow_control/if_let.html

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

# Closure

Doku:
https://doc.rust-lang.org/book/ch13-01-closures.html
https://doc.rust-lang.org/rust-by-example/fn/closures.html
https://doc.rust-lang.org/reference/types/closure.html

Die Parameter kommen zwischen die zwei `||` der Rückgabewert wird mit einem `->` gestartet und mit dem Typen beendet. Die Operation kommt zwischen `{ }`.
Vergleichbar einer Lambda Function von C#.

```rust
let closureTest = |x: i32| -> i32 {x * x};

let erg = closureTest(36);
```

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

# Ownership

Doku:
https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html


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

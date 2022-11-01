
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

# Structs

Doku:
https://doc.rust-lang.org/std/keyword.struct.html


# Schleifen

```rust
for i in variable {
	...
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
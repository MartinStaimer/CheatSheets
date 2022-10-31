
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

# Inhalt

[[RUST Snippets#Vector of u8 to string]]
[[RUST Snippets#String Parse]]
[[RUST Snippets#Hash Set Operations]]
[[RUST Snippets#Filter mit predicate (EQ => Linq C )]]
[[RUST Snippets#Fun with iterators]]
[[RUST Snippets#Standard Librarys]]
[[RUST Snippets#Arguments bzw. Parameter in Konsolenanwedung übergeben]]

# Vector of u8 to string

Doku:

https://doc.rust-lang.org/beta/core/str/fn.from_utf8.html
```rust
use std::str;

let buffer = vec![240, 159, 146, 150];

println!("{}", str::from_utf8(&buffer).unwrap());
```

[[RUST Snippets#Inhalt]]


# String Parse

Doku:
https://doc.rust-lang.org/std/primitive.str.html#method.parse

```rust
let str_number: &str = "85589";
let result = str_number.parse::<u32>().unwrap();
// Oder besser -> im Fehlerfall Standardwert
let result = str_number.parse::<u32>().unwrap_or(0);
println!("{}", result);
```

[[RUST Snippets#Inhalt]]

# Hash Set Operations

Doku:
intersection: https://doc.rust-lang.org/std/collections/hash_set/struct.Intersection.html
union: https://doc.rust-lang.org/std/collections/hash_set/struct.Union.html
diff: https://doc.rust-lang.org/std/collections/hash_set/struct.Difference.html

Intern: [[RUST#Hash Set]]

```rust
let hash_set1= HashSet::from([1, 3, 4, 5]);
let hash_set2= HashSet::from([2, 3, 5]);

// Intersection  
let intersection = (&hash_set1) & (&hash_set2);
let mut intersection_func = hash_set1.intersection(&hash_set2);

// Union
let union = (&hash_set1) | (&hash_set2);
let mut union_func = hash_set2.union(&hash_set1);
 
// diff
let diff = (&hash_set2) - (&hash_set1);
let diff2 = (&hash_set1) - (&hash_set2);
let mut dif_func = hash_set1.difference(&hash_set2);

// Print to Console
println!("{:?}", intersection);
println!("{:?}", intersection_func);

println!("{:?}", union);
println!("{:?}", union_func);

println!("{:?}", diff);
println!("{:?}", diff2);
println!("{:?}", dif_func);

// Ergebniss

// {3, 5}
// [3, 5]
// {5, 4, 3, 1, 2}
// [1, 3, 5, 4, 2]
// {2}
// {4, 1}
// [4, 1]
```

[[RUST Snippets#Inhalt]]


# Filter mit predicate (EQ => Linq C#)

Ers wird der Paramteter `Vec` in eine iteration zerlegt. jetzt wird über die Filterfunktion ein abgleich durchgeführt in diesem Fall `|val| val.contains("gruen")` und anschließend wieder zu einem `Vec` zusammengefasst.

```rust
// Filteroption ist hart kodiert
fn filter_green(data: &mut Vec<String>) -> Vec<&String> {
    data.iter()
        .filter(|val| val.contains("gruen"))
        .collect()
}

// Filteroption wird als Parameter übergeben
fn filter_green_match(data: &mut Vec<String>, filter_opt: String) -> Vec<&String> {
    data.iter()
        .filter(|val| val.contains(filter_opt.as_str()))
        .collect()
}

fn main() {

	let mut list_data: Vec<String> = vec![String::from("schwarz"),
                                          String::from("gelb"),
                                          String::from("gruen")];

    let filtered_green = filter_green(&mut list_data);
    
    let filtered_green_match = filter_green_match(&mut list_data, 
                                                  String::from("gruen"));

    println!("{:?}", filtered_green);
    println!("{:?}", filtered_green_match);
}
```

[[RUST Snippets#Inhalt]]

# Fun with iterators

## zip

Doku:
https://doc.rust-lang.org/stable/std/iter/fn.zip.html
https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/core/iter/trait.Iterator.html#method.zip

```rust
fn zipper(array_1: Vec<i32>, array_2: Vec<i32>) {
    for (val_1, val_2) in array_1.iter().zip(array_2.iter()) {
        println!("{:?},{:?}", val_1, val_2)
    }
}

fn main() {
    let test_vec = vec![1,2,3,4,5,6,7];
    let test_vec2 = vec![8,9,10,11,12]; 

    zipper(test_vec, test_vec2);
}

// Ausgabe
1,8
2,9
3,10
4,11
5,12
```


## For loop alternative -> for_each(closure)

Doku:
https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/core/iter/trait.Iterator.html#method.for_each

```rust
fn show_element(array_1: &Vec<i32>) {
    array_1.iter().for_each(|val| println!("{}", val))
}

fn main() {
    let test_vec = vec![1,2,3,4,5,6,7];
    show_element(&test_vec);
}

// Ausgabe
1
2
3
4
5
6
7
```


## Flatten -> baue aus mehrdimensionalen Arrays 1dimensionale

Doku:
https://doc.rust-lang.org/std/iter/struct.Flatten.html

```rust
fn flatten_arrays(array_1: &[[char; 5]; 2]) {
    array_1.iter().flatten().for_each(|val| print!("{}-", val))
}

fn main() {  
    let test_array = [['a','b','c','d','e'],['f','g','h','i','j']];

    flatten_arrays(&test_array);
}

// Augabe
a-b-c-d-e-f-g-h-i-j-
```

## Richtig kranker scheiß -> Ultimativer iter Baum

Hier wird ein Array mit Strings in eine Struct `Bestellung` geparst und anschließend gefiltert.
Nur Bestellungen über 4 Kg werden erfasst.

```rust
#[derive(Debug)]
struct Bestellung {
    pos: u32,
    frucht: String,
    kg: u32,
}

fn main() {  
    let bestellung_raw = ["1 Birne 50",
                          "2 Apfel 5",
                          "3 Zitrone 1"];

    let bestellung_pretty: Vec<Bestellung> = bestellung_raw.iter()
         .map(|val| {
	        let mut s = val.split(' ');
	        let pos = s.next()?.parse::<u32>().ok()?;
	        let frucht = s.next()?.to_string();
	        let kg = s.next()?.parse::<u32>().ok()?; 

	        Some(Bestellung{pos, frucht, kg})
		  })
	      .filter(|val| match val {
	        Some(v) => v.kg > 4,
	        None => false,
	      })
	      .flatten()
	      .collect();
 

    println!("{:#?}", bestellung_pretty);
}

// Ausgabe
[
    Bestellung {
        pos: 1,
        frucht: "Birne",
        kg: 50,
    },
    Bestellung {
        pos: 2,
        frucht: "Apfel",
        kg: 5,
    },
]

```

[[RUST Snippets#Inhalt]]

# Standard Librarys

`std::fs` -> Filesystem
https://doc.rust-lang.org/std/fs/
https://doc.rust-lang.org/rust-by-example/std_misc/fs.html

`std::env` -> Environment
https://doc.rust-lang.org/std/env/index.html
https://doc.rust-lang.org/book/ch12-05-working-with-environment-variables.html

`std::io::{Error, ErrorKind`} -> Fehlerunterstuetzung


[[RUST Snippets#Inhalt]]

# Arguments bzw. Parameter in Konsolenanwedung übergeben

```rust
use std::env;

fn main() {
    let arg: Vec<String> = env::args().collect();
}
```

[[RUST Snippets#Inhalt]]
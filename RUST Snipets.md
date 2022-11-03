# Inhalt

[[RUST Snipets#Vector of u8 to string]]
[[RUST Snipets#String Parse]]
[[RUST Snipets#Hash Set Operations]]
[[RUST Snipets#Filter mit predicate (EQ => Linq C )]]
[[RUST Snipets#Fun with iterators]]


# Vector of u8 to string

Doku:

https://doc.rust-lang.org/beta/core/str/fn.from_utf8.html
```rust
use std::str;

let buffer = vec![240, 159, 146, 150];

println!("{}", str::from_utf8(&buffer).unwrap());
```

[[RUST Snipets#Inhalt]]


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

[[RUST Snipets#Inhalt]]

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

[[RUST Snipets#Inhalt]]


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

[[RUST Snipets#Inhalt]]

# Fun with iterators

## zip

Doku:
https://doc.rust-lang.org/stable/std/iter/fn.zip.html

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

[[RUST Snipets#Inhalt]]
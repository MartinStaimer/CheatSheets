# Inhalt

[[RUST Snipets#Vector of u8 to string]]
[[RUST Snipets#String Parse]]
[[RUST Snipets#Hash Set Operations]]


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
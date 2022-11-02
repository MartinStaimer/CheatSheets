# Inhalt

[[RUST Cheats#Vector of u8 to string]]
[[RUST Cheats#String Parse]]


# Vector of u8 to string

Doku:

https://doc.rust-lang.org/beta/core/str/fn.from_utf8.html
```rust
use std::str;

let buffer = vec![240, 159, 146, 150];

println!("{}", str::from_utf8(&buffer).unwrap());
```

[[RUST Cheats#Inhalt]]


# String Parse

Doku:
https://doc.rust-lang.org/std/primitive.str.html#method.parse

```rust
let str_number: &str = "85589";
let result = str_number.parse::<u32>().unwrap();
println!("{}", result);
```

[[RUST Cheats#Inhalt]]
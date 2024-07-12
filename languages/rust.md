# Rust 

## Usefull website 

 - Official website : [https://www.rust-lang.org/learn/get-started](https://www.rust-lang.org/learn/get-started)
 - Rust book : https://doc.rust-lang.org/book/title-page.html
 - Rust by example : https://doc.rust-lang.org/rust-by-example/hello.html

## Cargo

Cargo is the Rust’s package manager and build system.

It does lots of things:
- build your project with cargo build
- run your project with cargo run
- test your project with cargo test
- build documentation for your project with cargo doc
- publish a library to crates.io with cargo publish


## Basics

### Running a program
- Build program
    ```
    rustc <FILE.rs>
    ```
- Create a project
    ```
    cargo new <PROJECT_NAME>
    ```
    This will create :
    - `src` folder for source code of the program
    - `Cargo.toml` file TOML (Tom’s Obvious, Minimal Language)
- Execute a project (in the root path) :
    ```
    cargo run
    ```

### Concepts

- by Defalt all variable are immutable

### Strings

```rust
let s: &str = "Hello world";
let s: String = "Hello world".to_string();
```

#### Focus on &str

```rust
let bstr : &str = "hellow.world";

bstr.split("."); -> iterator

bstr.contains("."); -> bool
```

#### Focus on String
```rust
let mut string : String = String::new();

string.push('c');
string.push_str("characters");

string.is_empty();

string.split_at(line.find(':').unwrap());

string.parse::<i32>().unwrap()

string.contains("."); -> bool

for char in string.chars() {
    println!("{}", char);
}

for (index, char) in string.chars().enumerate() {
    println!("Character at index {}: {}", index, char);
}

let chars: Vec<char> = s.chars().collect();
chars.get(4)
```


# Chapter 2
### New project
```bash
cargo new <project_name>
# Creating binary (application) `hello` package
# note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

tree hello
# hello
# ├── Cargo.toml
# └── src
#     └── main.rs
```


### Running new project
```bash
cd hello
cargo run
#    Compiling hello v0.1.0 (/home/bartosz/RustoverProjects/rust-learning/chapter_2/hello)
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
#      Running `target/debug/hello`
# Hello, world!

tree hello
#    hello
#    ├── Cargo.lock
#    ├── Cargo.toml
#    ├── src
#    │   └── main.rs
#    └── target
#        ├── .rustc_info.json
#        ├── CACHEDIR.TAG
#        └── debug
#            ├── .cargo-lock
#            ├── .fingerprint
#            │   ├── hello-1bfdcdd7b6a20833
#            │   │   ├── bin-hello
#            │   │   ├── bin-hello.json
#            │   │   └── invoked.timestamp
#            │   ├── hello-cb677569080a1c8e
#            │   │   ├── bin-hello
#            │   │   ├── bin-hello.json
#            │   │   ├── dep-bin-hello
#            │   │   └── invoked.timestamp
#            │   └── hello-d13f0aa782832758
#            │       ├── invoked.timestamp
#            │       ├── test-bin-hello
#            │       └── test-bin-hello.json
#            ├── build
#            ├── deps
#            │   ├── hello-cb677569080a1c8e
#            │   └── hello-cb677569080a1c8e.d
#            ├── examples
#            ├── hello
#            ├── hello.d
#            └── incremental
#                └── hello-f8qqejy0ax5l
#                    ├── s-gwtwz7nmc8-8fyc5z-a1fjcpplkt7l0fx8lj3zaeyh4
#                    │   ├── 1m9zuemq58pqvqic.o
#                    │   ├── 1p9l9n2bf0nc4xfi.o
#                    │   ├── 24xne1gstg27qcmb.o
#                    │   ├── 2eg8bhodjy6fg9c9.o
#                    │   ├── 3q9wc68f1n6d74dp.o
#                    │   ├── 43c2hnlg9cffe8qr.o
#                    │   ├── dep-graph.bin
#                    │   ├── query-cache.bin
#                    │   └── work-products.bin
#                    └── s-gwtwz7nmc8-8fyc5z.lock
#    
#    14 directories, 30 files
```

### Manual run
```bash
hello/target/debug/hello
# Hello, world!
```

### Remove and clean compilations
```shell
cargo clean
# Removed 27 files, 7.3MiB total
# directory target is removed

tree hello
#    hello
#    ├── Cargo.lock
#    ├── Cargo.toml
#    └── src
#        └── main.rs
#    
#    2 directories, 3 files
```

# Rust code structure
```rust
fn gdc(mut n: u64, mut m: u64) -> u64 {
    assert!(n != 0 && m != 0);
    while m != 0 {
        if m < n {
            let t = m;
            m = n;
            n = t;
        }
        m = m % n;
    }
    n // no semicolon at the end of the function - it is a return value
}
```
- `fn` - function definition
- `u64` type - 64bit unassigned integer (no negative)
- `->` define returned type
- `i32` type - 32 bit signed integer (with negative)
- `u8` type - 8 bit unassigned integer
- `isize` type - signed integer (depends on the platform 32 or 64 bit)
- `usize` type - unsigned integer (depends on the platform 32 or 64 bit)
- `f32` type - 32 bit floating point number (float)
- `f64` type - 64 bit floating point number (double)
- `mut` - mutable variable - once assigned can be changed
- no `mut` - immutable variable - once assigned cannot be changed
- `assert!` - macro that checks if the condition is true, if not it will panic. The ! at the end of the macro name indicates that it is a macro.
- `debug_assert!` - macro that checks if the condition is true, if not it will panic. It is only active in debug mode.
- `let` - keyword to define a local variable. Dont have to specify the type, Rust will infer it.

## Testing
```rust
#[test]
fn test_gcd() {
    assert_eq!(gcd(14, 15), 1);

    assert_eq!(gdc(2 * 3 * 5 * 11 * 17, 3 * 7 * 11 * 13 * 19), 3 * 11);
}
```

- `#[test]` - attribute that indicates that the function is a test function. The function name should start with `test_` or end with `_test`.
- `assert_eq!` - macro that checks if the two arguments are equal. If not it will panic.

### Run
```shell
cargo test
#   Compiling hello v0.1.0 (/home/bartosz/RustoverProjects/rust-learning/chapter_2/hello)
#    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.22s
#     Running unittests src/main.rs (target/debug/deps/hello-f81195a0fbf1eecf)
#
#    running 1 test
#    test test_gcd ... ok
#    
#    test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

```

## Running from terminal
```rust
use std::str::FromStr;
use std::env;

fn main() {
    let mut numbers = Vec::new();

    for arg in std::env::args().skip(1) {
        numbers.push(u64::from_str(&arg).expect("Błąd parsowania argumentu"));
    }

    if numbers.len() == 0 {
        eprintln!("Sposób wywołania: gcd LICZBA ...");
        std::process::exit(1);
    }

    let mut d = numbers[0];

    for m in &numbers[1..] {
        d = gdc(d, *m);
    }

    println!("Największy wspólny dzielnik {:?} to {}", numbers, d);

}
```
- `use std::str::FromStr;` - import `FromStr` trait from `std::str` module
- `use std::env;` - import `env` module from `std` library that contains functions to interact with the environment
- `std::env::args()` - function that returns an iterator over the command line arguments
- `std::env::args().skip(1)` - skip the first argument which is the name of the program
- `u64::from_str(&arg)` - parse the string to `u64` type
- `let mut numbers = Vec::new();` - create a new vector of `u64` type, like a dynamic array or list in Python
- `numbers.push(u64::from_str(&arg).expect("Błąd parsowania argumentu"));` - push the parsed number to the vector. If the parsing fails, it will panic with the message "Błąd parsowania argumentu"
- `eprintln!` - macro that prints to the standard error
- `std::process::exit(1);` - exit the program with the error code 1
- `&numbers[1..]` - slice of the vector from the second element to the end, & is a reference to the slice
- `*m` - dereference the value of the iterator
- `println!` - macro that prints to the standard output

### Run
```shell
cargo run 42 56 98
#        Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
#         Running `target/debug/hello 42 56 98`
#    Największy wspólny dzielnik [42, 56, 98] to 14
```

## Server WWW
- crates.io - repository of Rust packages
- [Framework Benchmarks](https://www.techempower.com/benchmarks/#section=data-r22&test=composite&hw=ph)
  - [1] ntex (rust)
  - [2] may-minihttp (rust)
  - [3] xitca-web (rust)
  - [13] actix-web (rust)
- serde - serialization and deserialization library

```rust
use actix_web::{web, App, HttpResponse, HttpServer};

fn main() {
    let server = HttpServer::new(|| {
        App::new()
            .route("/", web::get().to(get_index))
    });

    println!("Serwer dostępny pod adres http://localhost:3000...");
    server
        .bind("127.0.0.1:3000").expect("Błąd powiązania serwera z adresem")
        .run()
        .expect("Błąd uruchamiania serwera")
}

fn get_index() -> HttpResponse {
    HttpResponse::Ok()
        .content_type("text/html")
        .body(
            r#"
                <title>Kalkulator GCD</title>
                <form action="/gdc" method="post">
                    <input type="text" name="n"/>
                    <input type="text" name="m"/>
                    <button type="submit">Oblicz GCD</button>
                </form>
            "#
        )
}
```
- `actix_web::{web, App, HttpResponse, HttpServer}` - import the necessary functions and structures from the `actix_web` crate. Allows to use the functions without the crate name prefix.
- `HttpServer::new()` - create a new instance of the `HttpServer` structure
- `|| { App::new()... }` - closure that creates a new instance of the `App` structure. Equivalent to `fn() -> App { App::new()... }`or lambda function in Python.
- `.route("/", web::get().to(get_index))` - add a new route to the application. The first argument is the path, the second is the method, and the third is the handler function.
- `HttpResponse::Ok()` - create a new instance of the `HttpResponse` structure with the status code 200 (OK)
- `.content_type("text/html")` - set the content type of the response to text/html
- `.body()` - set the body of the response
- `r#""#` - raw string literal, allows to write multi-line strings without escaping the special characters


```rust
#[derive(Deserialize)]
struct GcdParameters {
    n: u64,
    m: u64,
}
```
- `#[derive(Deserialize)]` - derive the `Deserialize` trait for the structure. The `Deserialize` trait is used by the `serde` library to deserialize the structure from a string.
- `struct GcdParameters` - define a new structure with two fields `n` and `m` of the `u64` type

# Rust Learning

My Rust programming journey.

## Projects

### hello_cargo
First Rust project with Cargo

## Run

\`\`\`bash
cd hello_cargo
cargo run
\`\`\`


# Summary: Key Topics from the Rust Guessing Game

## Core Concepts Introduced

### 1. **Basic Rust Syntax**
- `fn main()` - program entry point
- `println!()` - macro for printing to console
- `//` - single-line comments

### 2. **Variables and Mutability**
- `let` - creates immutable variables by default
- `let mut` - creates mutable variables
- Shadowing - reusing variable names with `let` to convert types

### 3. **Input/Output Handling**
- `use std::io` - bringing the I/O library into scope
- `io::stdin().read_line(&mut guess)` - reading user input
- `&mut` - mutable references

### 4. **Result Type and Error Handling**
- `Result` enum with `Ok` and `Err` variants
- `.expect("error message")` - crashes on error with custom message
- `match` expressions for pattern matching on Result
- Using `continue` to ignore errors and retry

### 5. **External Crates (Dependencies)**
- Adding dependencies to `Cargo.toml` under `[dependencies]`
- `rand = "0.8.5"` - semantic versioning (^0.8.5 means 0.8.5 ≤ version < 0.9.0)
- `use rand::Rng` - bringing traits into scope
- `rand::thread_rng().gen_range(1..=100)` - generating random numbers
- `cargo doc --open` - viewing dependency documentation

### 6. **Cargo Commands**
- `cargo new` - creates new project
- `cargo run` - compiles and runs
- `cargo build` - compiles only
- `cargo update` - updates dependencies within version specifiers
- `Cargo.lock` - ensures reproducible builds

### 7. **String Manipulation**
- `String::new()` - creates empty growable string
- `.trim()` - removes whitespace and newlines (`\n`, `\r\n`)
- `.parse()` - converts string to another type
- Placeholders: `println!("You guessed: {guess}")`

### 8. **Comparison and Ordering**
- `use std::cmp::Ordering` - enum with variants `Less`, `Greater`, `Equal`
- `.cmp(&other)` - compares two values
- `match` expressions for handling comparison results

### 9. **Loops and Control Flow**
- `loop` - infinite loop
- `break` - exits loop when correct guess is made
- `continue` - skips to next iteration (handles invalid input)

### 10. **Type System**
- Strong static typing with inference
- Type annotation: `let guess: u32`
- `u32` - unsigned 32-bit integer (default for positive numbers)

## Key Patterns Used

| Pattern | Purpose |
|---------|---------|
| `match guess.trim().parse() { Ok(num) => num, Err(_) => continue }` | Safely handle potential parsing errors |
| `match guess.cmp(&secret_number) { Ordering::Less => ..., Ordering::Greater => ..., Ordering::Equal => { ... break } }` | Compare values with comprehensive handling |
| Shadowing `let guess: u32 = guess.trim().parse()...` | Convert between types while reusing variable name |

## Project Structure
```
guessing_game/
├── Cargo.toml      # Dependencies and project metadata
├── Cargo.lock      # Exact dependency versions (auto-generated)
└── src/
    └── main.rs     # Game code
```

## What Makes Rust Unique (Preview)
- Immutability by default
- References with explicit mutability (`&mut`)
- Powerful `match` expressions for exhaustive handling
- `Result` type forces error handling consideration
- Cargo's dependency management and reproducible builds

# Complete Rust Study Guide: From Beginner to Developer-Ready

Based on the official "The Rust Programming Language" book (The Book), this guide distills all essential concepts into a structured learning path.

---

## 📚 PART 1: RUST FUNDAMENTALS (Chapters 1-3)

### 1.1 Basic Syntax & Tooling
```bash
cargo new project_name
cargo build    # Compile
cargo run      # Compile + run
cargo check    # Fast check (no executable)
cargo test     # Run tests
cargo fmt      # Auto-format code
cargo clippy   # Linter
```

### 1.2 Variables & Mutability

| Concept | Syntax | Behavior |
|---------|--------|----------|
| Immutable (default) | `let x = 5` | Cannot change |
| Mutable | `let mut x = 5` | Can change |
| Constant | `const MAX: u32 = 100` | Compile-time, must annotate type |
| Shadowing | `let x = x + 1` | Re-declare with same name |

```rust
// Shadowing vs mut
let x = 5;
let x = x + 1;  // New variable (shadows) - type can change

let mut y = 5;
y = y + 1;      // Same variable - type cannot change
```

### 1.3 Data Types

#### Scalar Types (Single values)

| Type | Sizes | Range / Use |
|------|-------|-------------|
| **Integer** | `i8`, `i16`, `i32`, `i64`, `i128`, `isize` | Signed: -2ⁿ⁻¹ to 2ⁿ⁻¹-1 |
| | `u8`, `u16`, `u32`, `u64`, `u128`, `usize` | Unsigned: 0 to 2ⁿ-1 |
| **Default** | `i32` | Fast, balanced |
| **Float** | `f32`, `f64` | Default: `f64` |
| **Boolean** | `bool` | `true` / `false` |
| **Character** | `char` | 4 bytes, Unicode (`'😀'`) |

**Integer Overflow** (debug vs release):
- Debug: Panics on overflow
- Release: Two's complement wrapping

#### Compound Types
```rust
// Tuple (fixed length, mixed types)
let tup: (i32, f64, char) = (500, 6.4, 'z');
let (x, y, z) = tup;  // Destructuring
let one = tup.0;       // Index access

// Array (fixed length, same type)
let arr: [i32; 5] = [1, 2, 3, 4, 5];
let first = arr[0];
```

### 1.4 Functions
```rust
fn add(x: i32, y: i32) -> i32 {
    x + y  // Last expression is return (no semicolon!)
    // or return x + y; (with semicolon)
}

// Statements vs Expressions
let y = {          // Block is an expression
    let x = 3;
    x + 1          // No semicolon = value
};  // y = 4
```

### 1.5 Control Flow

```rust
// if-else (must be bool condition)
let number = if condition { 5 } else { 10 };

// loop - infinite
loop {
    break;  // Exit
    continue;
}

// while
while condition { }

// for (most common)
for i in 0..10 { }      // 0 to 9
for i in 0..=10 { }     // 0 to 10 (inclusive)
for item in &collection { }

// match (exhaustive!)
match value {
    1 => println!("one"),
    2..=10 => println!("2-10"),
    _ => println!("other"),  // Catch-all
}
```

---

## 🔑 PART 2: OWNERSHIP & BORROWING (Chapter 4)

### The Core Rules
1. Each value has one **owner**
2. When owner goes out of scope, value is dropped
3. Only one owner at a time

### Move vs Copy vs Clone

```rust
// Move (default for heap types)
let s1 = String::from("hello");
let s2 = s1;  // s1 is MOVED, can't use s1

// Copy (for stack types like integers, bool)
let x = 5;
let y = x;    // x still valid (implements Copy)

// Clone (explicit deep copy)
let s2 = s1.clone();  // Deep copy, expensive
```

### Ownership Rules by Type

| Type | Behavior | Implements Copy? |
|------|----------|------------------|
| Integers, bool, char | Copy | ✅ |
| Tuples of Copy types | Copy | ✅ |
| String, Vec, HashMap | Move | ❌ |
| Arrays of Copy types | Copy | ✅ |

### References & Borrowing

```rust
// Immutable reference (can have many)
let len = calculate_length(&s1);

// Mutable reference (can have only ONE)
let s = &mut s1;
change(s);

// Rules:
// - Cannot have mutable + immutable references simultaneously
// - References must always be valid (no dangling)
```

### The Slice Type

```rust
let s = String::from("hello world");
let hello = &s[0..5];   // "hello"
let world = &s[6..11];  // "world"
let whole = &s[..];     // "hello world"

// String literals are slices: &str
let literal: &str = "Hello";

// Best practice: Accept &str for string parameters
fn first_word(s: &str) -> &str { }
```

---

## 📦 PART 3: STRUCTS, ENUMS & MODULES (Chapters 5-7)

### Structs

```rust
// Named fields
struct User {
    active: bool,
    username: String,
    email: String,
}

// Tuple struct
struct Color(i32, i32, i32);

// Unit struct
struct AlwaysEqual;

// Instantiation
let user = User {
    email: String::from("someone@example.com"),
    ..other_user  // Struct update syntax
};

// Methods
impl User {
    fn area(&self) -> u32 {  // &self = self: &Self
        self.width * self.height
    }
    
    fn new(email: String) -> Self {
        Self { email, ..Default::default() }
    }
}

// Associated functions (like static methods)
impl User {
    fn default() -> Self { ... }
}
```

### Enums

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

enum Option<T> {  // Built-in, no null!
    Some(T),
    None,
}

enum Result<T, E> {  // Error handling
    Ok(T),
    Err(E),
}

// Using Option
let x: Option<i32> = Some(5);
let y: i32 = 5;
// let sum = x + y;  // ERROR! Must unwrap
let sum = x.unwrap_or(0) + y;
```

### Pattern Matching

```rust
match value {
    Some(5) => println!("five"),
    Some(x) if x > 5 => println!("big"),  // Match guard
    Some(x) => println!("{x}"),
    None => (),
}

// if let (single pattern)
if let Some(3) = value {
    println!("three");
}

// let else (requires exhaustive pattern)
let Some(value) = optional else {
    return;  // Must diverge (return/break/panic)
};
```

### Modules System

```rust
// In lib.rs or main.rs
mod front_of_house {
    pub mod hosting {  // pub for external access
        pub fn add_to_waitlist() {}
    }
}

// Paths
crate::front_of_house::hosting::add_to_waitlist();  // Absolute
self::front_of_house::...  // Relative
super::...  // Parent module

// Bringing into scope
use crate::front_of_house::hosting;
use std::collections::HashMap as Map;  // Aliasing
use rand::Rng as _;  // Import trait but not name (for traits)

// Re-exporting
pub use crate::front_of_house::hosting;

// Nested paths
use std::{cmp::Ordering, io};

// Glob
use std::collections::*;
```

### File Structure
```
src/
├── main.rs          # Binary crate entry
├── lib.rs           # Library crate root
├── front_of_house/
│   ├── mod.rs       # Module declaration
│   └── hosting.rs   # Module implementation
└── client.rs        # Separate module file
```

---

## 🗂️ PART 4: COLLECTIONS & ERROR HANDLING (Chapters 8-9)

### Common Collections

```rust
// Vec<T> - Growable array
let mut v: Vec<i32> = Vec::new();
v.push(5);
v.push(6);
let third = &v[2];        // Panic if out of bounds
let third = v.get(2);     // Returns Option<&i32>

// Macro
let v = vec![1, 2, 3];

// Iteration
for i in &v { }
for i in &mut v { *i += 10; }

// String (UTF-8)
let mut s = String::new();
let s = "initial".to_string();
let s = String::from("initial");
s.push_str(" world");
s.push('!');

// Concatenation
let s3 = s1 + &s2;  // s1 moved, s2 borrowed
let s = format!("{}-{}-{}", a, b, c);  // Doesn't take ownership

// Slicing strings (by byte boundaries - careful!)
let hello = "Здравствуйте";
let s = &hello[0..4];  // Panics if not char boundary

// HashMap
use std::collections::HashMap;
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.entry(String::from("Yellow")).or_insert(50);
let score = scores.get("Blue").copied().unwrap_or(0);
```

### Error Handling

```rust
// panic! - Unrecoverable
panic!("crash and burn");
// RUST_BACKTRACE=1 cargo run

// Result - Recoverable
enum Result<T, E> {
    Ok(T),
    Err(E),
}

fn read_file() -> Result<String, io::Error> {
    let mut file = File::open("hello.txt")?;  // ? propagates error
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
    // Shorthand: fs::read_to_string("hello.txt")
}

// The ? operator works in functions returning Result or Option
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}

// Error handling patterns
match result {
    Ok(val) => val,
    Err(e) => return Err(e.into()),  // Convert error types
}

// .unwrap() - panic on Err (use only in examples/tests)
// .expect("message") - panic with custom message
// .unwrap_or_default() - return default on Err
```

---

## 🔧 PART 5: GENERICS, TRAITS & LIFETIMES (Chapter 10)

### Generics

```rust
// In functions
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list {
        if item > largest {
            largest = item;
        }
    }
    largest
}

// In structs
struct Point<T> {
    x: T,
    y: T,
}

// Multiple types
struct Pair<X, Y> {
    first: X,
    second: Y,
}

// In enums
enum Result<T, E> {
    Ok(T),
    Err(E),
}

// In impl
impl<T> Point<T> {
    fn x(&self) -> &T { &self.x }
}

// Monomorphization: Compiler generates concrete versions at compile time
```

### Traits (Interfaces)

```rust
// Define trait
pub trait Summary {
    fn summarize(&self) -> String;
    
    // Default implementation
    fn summarize_author(&self) -> String {
        String::from("(Read more...)")
    }
}

// Implement
impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {}", self.headline, self.author)
    }
}

// Trait bounds
pub fn notify(item: &impl Summary) { }
// Desugared:
pub fn notify<T: Summary>(item: &T) { }

// Multiple bounds
pub fn notify(item: &(impl Summary + Display)) { }
// or
pub fn notify<T: Summary + Display>(item: &T) { }

// Where clause
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{ }

// Returning impl Trait
fn returns_summarizable() -> impl Summary {  // Single type only!
    NewsArticle { ... }
}

// Blanket implementations
impl<T: Display> ToString for T { }  // All Display types get to_string()
```

### Lifetimes (Prevent Dangling References)

```rust
// Lifetime annotations: 'a
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

// Lifetime elision rules (compiler infers):
// 1. Each parameter gets its own lifetime
// 2. If one input lifetime, output gets that lifetime
// 3. If &self or &mut self, output gets self's lifetime

// Structs with references
struct Excerpt<'a> {
    part: &'a str,
}

// Static lifetime (lives entire program)
let s: &'static str = "Hello";  // String literals

// Lifetime bounds
fn longest_with_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{ }
```

---

## 🧪 PART 6: TESTING & FUNCTIONAL FEATURES (Chapters 11-13)

### Testing

```rust
// Unit tests (in same file)
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }
    
    #[test]
    #[should_panic(expected = "divide by zero")]
    fn divide_by_zero() { ... }
    
    #[test]
    #[ignore]  // Expensive test
    fn expensive_test() { ... }
}

// Integration tests (tests/*.rs)
// tests/integration_test.rs
use my_crate;

// Running tests
cargo test -- --test-threads=1  # Single thread
cargo test test_name  # Filter
cargo test -- --ignored  # Run ignored
```

### Closures

```rust
// Closure syntax
let add_one = |x: u32| -> u32 { x + 1 };
let add_one = |x| x + 1;  // Type inference

// Capturing environment
let x = 4;
let equal_to_x = |z| z == x;  // Borrows x

// Moving ownership into closure
let y = vec![1, 2, 3];
let closure = move |z| z == y;  // y moved

// Fn traits
// FnOnce: consumes captured variables (once)
// FnMut: mutably borrows (can change)
// Fn: immutably borrows (can call multiple times)
```

### Iterators

```rust
let v = vec![1, 2, 3];

// Iterator adapters (lazy)
let v2: Vec<_> = v.iter()
    .map(|x| x + 1)
    .filter(|x| x > 2)
    .collect();

// Common methods
.iter()      // Immutable references
.iter_mut()  // Mutable references
.into_iter() // Takes ownership

// Consumer methods
let sum: i32 = v.iter().sum();
let any = v.iter().any(|&x| x > 2);
let found = v.iter().find(|&&x| x == 2);

// Custom iterators
struct Counter { count: u32 }
impl Iterator for Counter {
    type Item = u32;
    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}
```

---

## 🔒 PART 7: SMART POINTERS & OWNERSHIP PATTERNS (Chapter 15)

### Smart Pointers Overview

| Type | Location | Tracking | Mutability | Use Case |
|------|----------|----------|------------|----------|
| `Box<T>` | Heap | Single owner | Via `&mut` | Recursive types, trait objects |
| `Rc<T>` | Heap | Multiple owners | Immutable | Shared ownership (single-threaded) |
| `RefCell<T>` | Stack | Single owner | Runtime checks | Interior mutability |
| `Arc<T>` | Heap | Multiple owners | Immutable | Thread-safe shared ownership |

### Box<T> (Heap Allocation)

```rust
// Recursive type (List)
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use List::{Cons, Nil};
let list = Cons(1, Box::new(Cons(2, Box::new(Nil))));

// Trait object
let b = Box::new(5);
```

### Rc<T> (Reference Counted)

```rust
use std::rc::Rc;
let a = Rc::new(List::Cons(5, Rc::new(List::Nil)));
let b = List::Cons(3, Rc::clone(&a));
let c = List::Cons(4, Rc::clone(&a));

Rc::strong_count(&a);  // Reference count
```

### RefCell<T> (Interior Mutability)

```rust
use std::cell::RefCell;

struct MockMessenger {
    sent_messages: RefCell<Vec<String>>,
}

impl MockMessenger {
    fn send(&self, message: String) {
        self.sent_messages.borrow_mut().push(message);
        // borrow() for immutable, borrow_mut() for mutable
    }
}

// Runtime checking: borrow rules enforced at runtime (panics if violated)
```

### Combined Patterns

```rust
// Rc<RefCell<T>> - Multiple owners who can mutate
use std::{rc::Rc, cell::RefCell};

let value = Rc::new(RefCell::new(5));
let a = Rc::clone(&value);
let b = Rc::clone(&value);

*a.borrow_mut() += 10;  // All see the change
```

### Drop Trait

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping: {}", self.data);
    }
}

std::mem::drop(x);  // Explicit early drop
```

---

## 🚀 PART 8: CONCURRENCY & ASYNC (Chapters 16-17)

### Threads

```rust
use std::thread;
use std::time::Duration;

// Spawn thread
let handle = thread::spawn(|| {
    for i in 1..10 {
        println!("Thread: {}", i);
        thread::sleep(Duration::from_millis(1));
    }
});

handle.join().unwrap();  // Wait for thread

// move closure for data ownership
let v = vec![1, 2, 3];
let handle = thread::spawn(move || {
    println!("{:?}", v);
});
```

### Message Passing (Channels)

```rust
use std::sync::mpsc;  // multiple producer, single consumer

let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    tx.send(42).unwrap();
});

let received = rx.recv().unwrap();  // Blocking
// or rx.try_recv() for non-blocking

// Multiple producers
let tx1 = tx.clone();
```

### Shared-State Concurrency (Mutex)

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    let handle = thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    });
    handles.push(handle);
}

// MutexGuard releases lock when dropped
```

### Async/Await

```rust
use tokio;  // Most common runtime

#[tokio::main]
async fn main() {
    let result = fetch_data().await;
}

async fn fetch_data() -> String {
    // .await yields control back to runtime
    let data = read_file().await;
    process_data(&data).await
}

// Concurrent execution
let (result1, result2) = tokio::join!(async1(), async2());

// Spawning tasks
let handle = tokio::spawn(async {
    do_work().await
});
handle.await.unwrap();

// Streams (async iterators)
use futures::stream::{StreamExt, iter};
let stream = iter(vec![1, 2, 3]);
stream.for_each(|item| async move {
    println!("{}", item);
}).await;
```

### Send and Sync Traits (Auto traits)

- `Send`: Safe to transfer ownership to another thread
  - Most types are Send, except `Rc<T>`
- `Sync`: Safe to share references across threads
  - `Mutex<T>` is Sync, `RefCell<T>` is not

---

## 🏗️ PART 9: ADVANCED FEATURES (Chapters 18-20)

### Patterns & Matching

```rust
// Match guards
match x {
    Some(y) if y < 5 => (),
    _ => (),
}

// @ bindings
match x {
    Some(y @ 1..=5) => println!("{}", y),
    _ => (),
}

// Range patterns
match x {
    1..=5 => (),
    _ => (),
}

// Struct patterns
let Point { x, y } = point;
let Point { x: x1, y: y1 } = point;
let Point { x, .. } = point;  // ignore rest

// Refutability
// Irrefutable: always matches (let, function args)
// Refutable: may fail (if let, while let)
```

### Unsafe Rust

```rust
// Unsafe superpowers:
// 1. Dereference raw pointers
// 2. Call unsafe functions
// 3. Access/modify mutable static variables
// 4. Implement unsafe traits
// 5. Access union fields

let mut num = 5;
let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;

unsafe {
    println!("{}", *r1);  // Raw pointer dereference
}

// Unsafe trait
unsafe trait Foo { }
unsafe impl Foo for i32 { }

// FFI (C interop)
extern "C" {
    fn abs(input: i32) -> i32;
}
```

### Macros

```rust
// Declarative macros (macro_rules!)
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}

// Procedural macros
// 1. Custom derive: #[derive(MyTrait)]
// 2. Attribute-like: #[route(GET, "/")]
// 3. Function-like: sql!(SELECT * FROM posts)
```

---

## 📦 PART 10: CARGO & ECOSYSTEM (Chapter 14)

### Cargo Features

```toml
[package]
name = "my_crate"
version = "0.1.0"
edition = "2021"

[dependencies]
rand = "0.8.5"
regex = { version = "1.5", optional = true }

[dev-dependencies]
pretty_assertions = "1.0"

[build-dependencies]
cc = "1.0"

[features]
default = ["regex"]
json = ["serde_json"]
```

### Workspaces

```toml
# Cargo.toml in root
[workspace]
members = [
    "adder",
    "add_one",
]

resolver = "2"
```

### Publishing

```bash
cargo login   # API token
cargo publish
cargo yank --vers 0.1.0  # Remove version
cargo install cargo-edit  # Install binary
```

---

## 🎯 FINAL PROJECT: Multithreaded Web Server (Chapter 21)

### Key Patterns Demonstrated

```rust
// Thread pool pattern
struct ThreadPool {
    workers: Vec<Worker>,
    sender: mpsc::Sender<Job>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool {
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));
        
        let mut workers = Vec::with_capacity(size);
        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }
        
        ThreadPool { workers, sender }
    }
    
    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.send(job).unwrap();
    }
}

// Graceful shutdown
impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.clone());  // Close channel
        
        for worker in &mut self.workers {
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}
```

---

## ✅ Developer Readiness Checklist

### Fundamentals (Must Know)
- [ ] Ownership, borrowing, lifetimes
- [ ] Pattern matching with match and if let
- [ ] Result and Option handling
- [ ] Structs, enums, traits
- [ ] Vector, String, HashMap usage
- [ ] Error handling with ? and custom errors
- [ ] Writing and running tests

### Intermediate (Regular Use)
- [ ] Generic types and trait bounds
- [ ] Closures and iterators
- [ ] Smart pointers (Box, Rc, RefCell)
- [ ] Module system and visibility
- [ ] Cargo workspaces and dependencies
- [ ] Async/await with tokio
- [ ] Threads and channels

### Advanced (Standout Skills)
- [ ] Building libraries and publishing
- [ ] FFI and unsafe Rust (when needed)
- [ ] Custom derive macros
- [ ] Concurrency patterns (Arc<Mutex>)
- [ ] Performance optimization
- [ ] Building CLI tools with clap

### Project Portfolio Ideas
1. **CLI Tool**: File search, grep clone, todo manager
2. **Web Server**: Actix-web or axum API with auth
3. **Game**: Terminal-based RPG using ratatui
4. **System Tool**: Process monitor, port scanner
5. **Parser**: JSON parser, markdown processor

### Recommended Crates by Category

| Purpose | Crates |
|---------|--------|
| Async runtime | tokio, async-std |
| Web framework | axum, actix-web, rocket |
| Serialization | serde, serde_json |
| CLI | clap, structopt, anyhow |
| HTTP client | reqwest |
| Database | sqlx, diesel |
| Logging | tracing, log, env_logger |
| Testing | pretty_assertions, proptest |
| Random | rand |
| Regex | regex |
| Dates | chrono |
| Parallelism | rayon |

---

## 🎓 Final Advice

1. **Read The Book** completely - it's excellent
2. **Use Rust Playground** for quick experiments
3. **Run Clippy** regularly - `cargo clippy -- -D warnings`
4. **Read source code** of popular crates (tokio, serde, axum)
5. **Build projects** - the best way to learn
6. **Join community** - users.rust-lang.org, /r/rust

**You are now developer-ready!** Start building, contribute to open source, and enjoy the powerful, safe, and fast world of Rust. 🦀

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
